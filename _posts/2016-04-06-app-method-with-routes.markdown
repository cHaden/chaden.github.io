---
layout: post
title:  "Testing routes with the app method in Rails console"
date:   2016-04-06 12:43:00 -0400
categories: ruby stuff
---

Another favorite thing of the day:

Using `app.` in the Rails console to test out routes.

For example, if you add `resources :risky_rules` to your routes file in the `admin`
namespace, and want to test a path like `admin_risky_rules_path`, try
`app.admin_risky_rules_path` in the rails console. If it's a real working path, it will return
something like `=> "/admin/risky_rules"`, and, if your path doesn't exist, Rails console will give you an error.

This works for more involved routes too, such as slugged URLs and paths that take parameters!

For reference, [this blog post][blog-post] has lots of other exciting magic available with `app`:

[blog-post]: https://signalvnoise.com/posts/3176-three-quick-rails-console-tips
