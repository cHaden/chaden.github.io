---
layout: post
title:  "rake routes for a single controller"
date:   2016-05-04 11:43:00 -0400
categories: ruby stuff
---

If you have a lengthy routes file, running `rake routes` can spit out more text
than is needed for the task at hand. Often, I only really want to see the routes
for a single controller. In the past, I've just used `grep` to pare down the results,
but there is a built-in way to do this too:

For example, if the controller in question is `users_controller.rb`, you would type
`CONTROLLER=users rake routes`

If the controller in question is in a directory nested within the controllers directory,
include the name of the directory:

Example: to get the routes for `admin/users_controller.rb`, use
`CONTROLLER=admin/users rake routes`

#### Resources:
[Rails Routing from the Outside In][routing]

[routing]: http://guides.rubyonrails.org/routing.html
