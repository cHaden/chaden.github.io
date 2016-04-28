---
layout: post
title:  "Troubleshooting migrations"
date:   2016-04-15 11:43:00 -0400
categories: ruby stuff
---

Sometimes migrations get out of sync.

Use `rake db:migrate:status` to display status of migrations.

![rake migrate status screenshot]({{site.url}}/assets/pics/migrate_status.png)

If you need to run a specific migration, `rake db:migrate VERSION=<timestamp>`, where `<timestamp>`
is the timestamp of the specific migration that you want to run. (Example: "20160420195154")
