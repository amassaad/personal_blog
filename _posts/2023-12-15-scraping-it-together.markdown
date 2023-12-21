---
layout: post
title:  "Scraping Marketing Leads using Ruby and Selenium"
date:   2023-12-14 17:30:51 -0500
categories: ruby scraping selenium marketing
---
Over my life I've started (and stopped!) dozens of small businesses and side hustles. Central to each business venture is finding marketing leads. In some cases this is simple. For example when I was a magician I would market locally in party supply stores and online. By forming a relationship with this business I was able to leave business cards to hopefully get phone calls for business, and to definitely date me today. But how about a local business that needs to actively reach out and market themselves online? There are some very clever things we can do.

In this case my approach is to find clients from local business directories. In my area there are several public listings for local businesses, but manually capturing this data would be exhausting. I'm a software creator, can I not create something to help with this problem? Yes.

I'm going to teach how I was able to capture it all automatically using Ruby in under 2 hours.

![](/assets/img/overwhelming_leads.png)

### 49 Pages!? That's a bit too overwhelming to manually process.

This is where Ruby comes to the rescue!

Specifically I chose Selenium Webdriver wrapped in a Ruby script that I wrote in the root directory of a new Rails project.

## Design Choices

Sometimes the tool choice can impact the final result as much or even more than the actual effort and design, so why did I decide on these tools? Here is my reasoning:

1. Simplicity: Ruby is easy to read and this means its easier to write good code. Good code is easy change and this may need changing a year or two down the road (in case I want to update my data). I feel this will be very easy to adjust as the HTML changes.

1. Speed of Setup: All the gems are included by default in a new Rails 7 project. This makes it an absolute breeze to start development on my new project/experiment/great idea. As with any creative endeavour we are constantly battling [resistance](https://en.wikipedia.org/wiki/Resistance_(creativity)) which means getting started efficiently may mean the difference between this getting completed or not. All we have to do is: `rails new contact_builder`. I don't need any rails features, but it makes the getting started process very easy. I know that I can always cut the ruby code out and create a new Gemfile once the whole thing works.

1. Familiarity: I spent a decade at Shopify using Ruby to build things over the internet. Shopify is a [pretty large Ruby shop](https://github.com/Shopify?q=&type=all&language=ruby&sort=) and has showered me with an immense amount of information about this language. Go with what you know, in this case it will make me faster at coding this up.

1. Visual feedback: Using Selenium allows me to visually inspect what the browser is doing at each phase of the script. Rather than focussing exclusively on console logs I can augment this with the actual webpage I'm trying to scrape. Its much easier to reason about where in the script is when you can see the page open in Chrome.

## Hands to Keyboard

Where to begin? Thankfully when running a Selenium Webdriver script it will open a browser window so if you're attentive you will see a hint of what it is doing before the script auto-exits. The first thing I add is the main page, some boilerplate to require and grab a Selenium WebDriver instance and a sleep after navigating to the main page.

```ruby
require 'selenium-webdriver'

options = Selenium::WebDriver::Chrome::Options.new
@driver = Selenium::WebDriver.for :chrome, options: options

def scrape_pages
  root_url = 'https://example.com'
  retries = 0

  @driver.navigate.to root_url
  sleep 5

  @driver.quit
  end
end
```

### Simple enough, but this doesn't do anything - why!?

Start simple. So many things might go wrong that I try to make sure each step works before advancing. This ensures that when something does go wrong, you can rule out these more basic stages.

## Divide and Conquer

When coaching and mentoring I try to encourage a smaller, incremental approach where it makes sense. In this case I mean: rather than grabbing all my business info I will first generate a CSV with each detail page URL.

Here is what that would look like:

![](/assets/img/divide_responsabilties.png)

We have a directory index with a link to the next page, and links to details about each business. Rather than traverse each child now (especially while perfecting the script, and encountering failure) wouldn't it be better to split this into two steps? This is the approach I took.


```ruby
require 'selenium-webdriver'
require 'csv'
options = Selenium::WebDriver::Chrome::Options.new
@driver = Selenium::WebDriver.for :chrome, options: options

def scrape_pages
  root_url = 'https://directory.example.com/'
  retries = 0

  business_details_link_text = '' # Add the text for the link to the details here.
  next_page_class = '' # Add the class for the "next" page button here.
  loop do
    begin
      @driver.navigate.to root_url
      @driver.find_elements(partial_link_text: business_details_link_text).each do |link|
        CSV.open("pages.csv", "a+") do |csv|
          csv << ["url", "completed"] if csv.count.eql? 0
          csv << ["#{link.attribute('href')}", "false"]
        end
      end
    
      root_url = @driver.find_element(class: next_page_class).attribute('href')
      puts "Next url, sleeping for 2 seconds..."
      sleep 2
      retries = 0 # reset retries after successful scrape

    rescue Selenium::WebDriver::Error::NoSuchElementError, Net::ReadTimeout => e
      retries += 1
      if retries <= 3
        puts "Attempt #{retries} failed, retrying..."
        retry
      else
        puts "Failed to scrape data from #{root_url} after 3 attempts"
        break
      end
    end
  end

  @driver.quit
end
```

In this code I'm adding the csv library so I can save each page's details to disk. This also lets me resume the work in case it fails part way through. Next I find the link text for the details of the business and the CSS class for the next page link and save them as strings. 

I have wrapped the loop work within a begin/rescue clause because there might be a situation where the network fails, or Selenium can't find the element in question. I'm sure there are more edge cases to account for but these were the main bugs I encountered where I knew I wanted to just retry. 

Within the loop I open up a CSV with a simple format: url and completed status.

| url   | completed  |
| ----- | -----------|
| https://directory.example.com/Index/PageStructure/biz-name | false |

## On to the finer details

With my first csv in hand (or on disk . . . or in memory?!) I'm ready to tackle the details. 

Your needs may vary but I'd like to save my business info into a simple data structure that looks like this:

```ruby
contact = {
  business_name: '',
  address: '',
  phone: '',
  email: '',
  website: '',
  social: '',
  contact_person: '',
  contact_title: '',
}
```

I'm happy smashing all social media links into one field. I can always process the CSV a third time offline but the essential role of this step is to get the details off the internet and into a spreadsheet.

As a starting point I open the CSV from the previous step (after making a copy locally) and iterate through each page, choosing to skip to the next record if the completed column is set to `true`. The next part is a lot more complicated to explain - thankfully we split this up from the earlier part!

The details of the business directory each have a div which looks like this, in particular the business details are in an unnamed span following a named span for eg `<span id="Address">Address</span>` which is perfect for selecting with Selenium but terrible for capturing the data form the next element, or is it?

### HTML in question, notice how the address, phone etc are in unnamed divs. Darn!

```html
<div class="col-md-12">
  <dl class="some-class">
    <dt>
      <span id="Address">Address</span>
    </dt>
    <dd>
      123 Anytown <br />
      New York, New York<br />
    </dd>

    <dt>
      <span id="Phone">Phone:</span>
    </dt>
    <dd>
      555.555.5555
    </dd>

    <dt>
      <span id="Email">Email:</span>
    </dt>
    <dd>
      <a href="mailto:example@example.com" >email text</a>
    </dd>

    <dt>
      <span id="Website">Website:</span>
    </dt>
    <dd>
      <a href="http://www.example.com" target="_blank">http://www.example.com</a>
    </dd>

    <dt>
      <span id="Details">Business details:</span>
    </dt>
    <dd>
       <p>Here are the details</p>
    </dd>
  </dl>
</div>
```

The rest of the script tries to find business names (if they exist, not all are listed), then it looks at the table of business details as a set of dt and dd pairs. Collecting the table this way allows us to check which element we're looking at based on the text of the previous div. How handy!

The rest of the script is quite simple now. Each detail is saved into the contact data structure (if those details exist) and the entire contact is saved into the details.csv. If it is the first line in the CSV we write the keys (headers) rather than the values.

Finally if we get to this stage we save the status as completed in the original csv.

The script closes with some error catching, retry logic and completion notification.

There is one small bug, at the end it repeats the final page a few times and saves this data into the CSV. I manually removed it but this could of course be improved within the code.

```ruby
def scrape_details
  # Load the list of pages from the csv file
  pages = CSV.read('pages.csv', headers: true)

  pages.each do |p|
    next if p['completed'] == 'true' # skip if already completed
    url = p['url']
    puts "Navigating to #{url}"
    retries = 0

    begin
      contact = {
        business_name: '',
        address: '',
        phone: '',
        email: '',
        website: '',
        social: '',
        contact_person: '',
        contact_title: '',
      }
      
      @driver.navigate.to url
      # sleep 2

      # This part will need to be customized for each site
      business_name, address, phone, email, website, contact_person, contact_title, social = nil
      begin
        business_name = @driver.find_element(:css, '#id-if-this-exists').text
      rescue Selenium::WebDriver::Error::NoSuchElementError
      end
      
      begin
        contact_person = @driver.find_element(:css, '.tablename.class td:nth-child(1)').text
      rescue Selenium::WebDriver::Error::NoSuchElementError
      end
      
      begin
        contact_title = @driver.find_element(:css, '.tablename.class td:nth-child(2)').text
      rescue Selenium::WebDriver::Error::NoSuchElementError
      end
      
      # This part grabs the table using the text. We could also have used the ID
      dl = @driver.find_element(:css, '.dl-horizontal')
      dt_dd_pairs = dl.find_elements(:xpath, './/dt | .//dd')
      
      dt_dd_pairs.each_with_index do |element, i|
        begin
          case element.text.strip
          when 'Address:'
            address = dt_dd_pairs[i+1].text
          when 'Phone:'
            phone = dt_dd_pairs[i+1].text
          when 'Email:'
            email = dt_dd_pairs[i+1].find_element(:tag_name, 'a').attribute('href').gsub('mailto:', '')
          when 'Website:'
            website = dt_dd_pairs[i+1].find_element(:tag_name, 'a').attribute('href')
          when 'Social:'
            social = dt_dd_pairs[i+1].find_elements(:tag_name, 'a').map { |a| a.attribute('href') }.join(', ')
          end
        rescue Selenium::WebDriver::Error::NoSuchElementError
          next
        end
      end

      puts business_name, address, phone, email, website, contact_person, contact_title, social

      # sleep 2
      contact[:business_name] = business_name if business_name
      contact[:address] = address if address
      contact[:phone] = phone if phone
      contact[:email] = email if email
      contact[:website] = website if website
      contact[:contact_person] = contact_person if contact_person
      contact[:contact_title] = contact_title if contact_title
      contact[:social] = social if social

    CSV.open("details.csv", "a+") do |csv|
      csv << contact.keys if csv.count.eql? 0
      csv << contact.values
    end
    
    # Mark as completed in the original CSV after successful scraping
    p['completed'] = true
    CSV.open("pages.csv", "w+") do |csv|
      csv << pages.headers
      pages.each do |row|
        csv << row
      end
    end
    
  rescue Selenium::WebDriver::Error::NoSuchElementError, Net::ReadTimeout => e
    retries += 1
    if retries <= 3
      puts "Attempt #{retries} failed, retrying..."
      retry
    else
      puts "Failed to scrape data from #{url} after 3 attempts"
    end
  end

  rescue Selenium::WebDriver::Error::NoSuchElementError
    puts 'Done finding each contact details!'
    break
  end

  @driver.quit
end

scrape_pages
scrape_details
```

And there we go! An hour later I had a CSV with as many business details for local businesses as I could find. 
