---
layout: post
title: "Zen and the Art of Infinite Support Loops: My 17-Day Battle to Cancel Zendesk"
date: 2025-12-16 10:00:00 -0400
categories: customer-support zendesk
---

They say irony is dead, but I'm pretty sure it's just stuck in a ticket queue somewhere at Zendesk.

Recently, I decided to do some digital spring cleaning. I've had less time for side-projects lately, and you know the old saying: the nail that sticks out gets hammered down. For me, that nail was an old Zendesk developer account, a relic from a long-forgotten failed app that I didn't need anymore. I only noticed it was still active because, ironically, it started spamming me with a ridiculous amount of emails.

"No problem," I thought. "Zendesk is *the* industry leader in customer support software. Surely, using Zendesk to get support from Zendesk to cancel Zendesk will be a seamless experience." I thought.

Holy heck, I was so wrong!

What followed was 17 days, multiple agents, an AI chatbot named "Zea," and a Kafka-esque journey through the bowels of enterprise support infrastructure. I experienced it all, from A to Zea. Here is the chronicle of my descent into customer service purgatory.

---

## Phase 1: The "Managed" Trap

It started innocently on November 29th. I had recieved a few dozen spammer/junk notifications from my old Zendesk. I logged in, looking for a simple "Cancel Account" button. Instead, I was informed my account was "managed" and I'd need to contact support to make any changes.

*"Contact Zendesk Customer Support to make changes to your account."*

Enter Zea, Zendesk's AI agent.

![Zea AI Chat - The AI agent cheerfully offers to help me WATCH NEXT LEVEL CX WEBINAR SERIES while I'm trying to escape](/assets/img/blog/s1.png)

Zea was *very* eager to help me "WATCH NEXT LEVEL CX WEBINAR SERIES" or "LEARN ABOUT ZENDESK." When I typed "cancel," I was met with a menu of options: none of which were "cancel." I had to navigate through "Manage subscription" → wait for another menu → type "cancel" again → get told it's a managed account → agree to speak to a human → answer *more* qualifying questions about my request.

It's 7:40 AM. I just want to click an off button. The AI wants to know if this is about billing or technical support. *It's about leaving, Zea.*

---

## Phase 2: The Warm Hand-off into the Cold Void

Finally, a human! On November 30th, Jeric from the "Zendesk Advocacy Team" entered the chat. Relief washed over me.

Then I read his message:

![Jeric's response - He thinks I want to "cancel or change the message" about it being a managed account](/assets/img/blog/s2.png)

"I understand that you're looking to cancel or change the message stating, 'This is a managed account.'"

No, Jeric. I don't want to change the *message*. I want to cancel the *account*. The message is fine. The account's existence is the problem.

He then asked me to "share more about your goal for updating your account management" and "what specific changes are you hoping to make."

*I am hoping to make the account not exist anymore, Jeric.*

![The handoff - Jeric passes me to "the team which handles cancellation requests"](/assets/img/blog/s3.png)

Eventually, he understood and said he would "connect me over to our team, which handles cancellation requests." Great! A specialist team! I assumed this would take, maybe, an hour.

Four days passed. Silence.

On December 4th, I nudged them. The ticket looked like it was auto-closing due to inactivity. *I* was inactive? I literally replied "Hi there, I'm looking to cancel" to make sure there was no ambiguity.

*Sidenote: I want to be clear: I'm not dunking on individual support agents here. My first role at Shopify back in February 2013 was as a support agent. I've answered the angry phone calls, the frustrated emails, the impatient chats. I commiserate. I've made my share of mistakes under time pressure: like the time I addressed a miffed "Carla" as "Carlos." I get it. What I'm critiquing is the system, not the people trapped inside it.*

---

## Phase 3: Who Are You Again?

This is where it got truly Kafkaesque.

Rachel from the "Renewals team" finally responded. After being in an email thread associated with my account for five days, a thread where I'm logged in, where my email is visible, where the entire context is about canceling *my* account, she asked:

"Could you kindly confirm the subdomain you are requesting to cancel?"

![Rachel asking for subdomain confirmation - after 5 days in a thread about my account](/assets/img/blog/s4.png)

I stared at my screen. You are Zendesk. I am emailing you through Zendesk. About my Zendesk account. Which you can see. Because you are Zendesk.

By this point, I was responding from my iPhone in increasingly terse messages: "Perhaps something went wrong. I got this message that the ticket will be closed but I am waiting for a response. Trying to cancel this account. Thanks."

The replies were coming from different time zones. I was waking up to messages sent at 2:05 PM CST. 1:25 AM CST. 11:40 PM CST. This ticket was circumnavigating the globe while going absolutely nowhere, running in place.

---

## Phase 4: Spam Inception

By December 9th - *ten days* into this saga - I had reached my breaking point. The irony had become so thick you could cut it with a knife:

**While I was waiting for Zendesk to cancel my account to stop the spam notifications, Zendesk's system was sending me more spam notifications about the support ticket I opened to stop the spam.**

![My frustrated message showing the spam I'm receiving while trying to cancel](/assets/img/blog/s5.png)

I literally had to copy-paste their own system spam back to them as evidence. "Here is a sample of the spam I am still receiving from your systems," I wrote, including a forwarded notification about a ticket from some gibberish email address that my *still-active* Zendesk account was dutifully alerting me about.

"I keep getting passed around, can someone complete this task please?"

Meanwhile, Rachel responded: "I am still waiting for confirmation regarding the account cancellation."

Waiting for confirmation *from whom*? I had confirmed. Multiple times. Through multiple channels. Was there a secret council that needed to convene? A board of directors vote? Did the Finance team need to consult the ancient scrolls?

---

## The Resolution

Finally, on December 16th at 8:13 AM (seventeen days after I first typed "cancel" into a chat window) the golden email arrived:

![The final resolution - Finance has completed the cancellation](/assets/img/blog/s6.png)

"We would like to inform you that the Finance team has completed the cancellation of your account with the subdomain: d3v-atam."

*The Finance team.* It took escalation to *Finance* to flip a boolean from `active: true` to `active: false`.

Seventeen days. Four agents (Zea, Jeric, Rachel, and the mysterious Finance team). Countless emails. Multiple time zones. All to delete a developer sandbox account.

---

## The Takeaway

There's a particular flavor of dystopian comedy in watching a customer support software company struggle with customer support. It's like watching a fire extinguisher factory burn down, or a locksmith locked out of their own shop.

If Zendesk (the company that literally sells the tools to handle these interactions efficiently) can't manage a simple cancellation request without a 17-day odyssey through their own labyrinthine processes, what hope is there for the rest of us?

So the next time you feel bad about your own company's support response times, take heart: Even the people who build the customer experience infrastructure can't figure out how to deliver a good customer experience.

At least the webinar series is still available, I guess.

---

*Shameless plug: At [Victoria Garland](https://victoriagarland.ca/), we design software that's actually human-focused and easy to use, the kind where "cancel account" is a button, not a 17-day expedition. I'm the CTO, and if you're building something and want to avoid putting your users through support purgatory, [let's chat](https://victoriagarland.ca/).*
