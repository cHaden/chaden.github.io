---
layout: post
title:  "Using rake in production"
date:   2016-04-26 11:43:00 -0400
categories: ruby stuff
---

Using `rake` tasks in production:

Use `RAILS_ENV=production bundle exec rake <name of task>` instead of just `rake`
This makes sure to load the correct database config, and using `bundle exec` ensures
that the same gems that are in your Gemfile are used when you use `rake`

Also, be sure that the current working directory is in fact your app's directory.
