---
layout: post
title:  "Double Bang (!!)"
date:   2016-05-24 11:43:00 -0400
categories: ruby stuff
---

When you absolutely, positively need to get a boolean back.

This returns the truthy or falsey value of an expression as a boolean. Great for
avoiding the ambiguity of `nil`!

Check it out...
{% highlight ruby %}
a = nil
!!a #false
a = 1
!!a #true
a = 0
!!a #true
a = "potato"
!!a #true
a = false
!!a #false
{% endhighlight %}
