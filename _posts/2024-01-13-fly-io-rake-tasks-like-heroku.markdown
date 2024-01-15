---
layout: post
title: "Heroku Scheduler style rake tasks on fly.io"
date: 2024-01-15 09:16:51 -0500
categories: ruby heroku fly rake
---

This year I've started migrating many of my [personal software](https://github.com/amassaad?tab=repositories) projects from Heroku to fly.io to try and keep my costs down. One of the major pain points with Heroku is the pricing of their managed database service Heroku Postgres. Don't get me wrong, its great but it also adds up quickly. For example a tiny, anemic instance is available for $5/month and a slightly more moderate basic instance is available for $9/month. This adds up quick if you're like me and launch a few apps every month.

The migrating from Heroku to fly is quite simple and they have [excellent docs](https://fly.io/docs/rails/getting-started/migrate-from-heroku/) to help you get to the finish line. There was one aspect of this migration that I was not able to easily figure out was scheduled tasks. One of the more positive aspects is their pricing structure which is more pay-as-you-go vs the minimum payments Heroku requires.

Heroku has a very nice feature called the [Heroku Scheduler](https://elements.heroku.com/addons/scheduler) that allows you to run jobs with a cron-like regularity but I was not able to grok the docs to figure out how to do something similar on fly.io

Eventually I did find a blog post announcing a schedule option for fly machines, but the code examples were not clear. It wasn't immediately clear how to set an hourly task, but eventually I found the following syntax:

`fly machine run . --dockerfile Dockerfile bin/rails data:fetch --schedule daily`

This is run from the repo where the fly app was launched and references the Dockerfile to build the app. It also uses `bin/rails data:fetch` which when added after the Dockerfile replaces the entry command from the Dockerfile. This runs a one-off machine to run the task on a daily basis. This also allows me to use the same generated file that runs the rails app, while skipping the booting of the server.
