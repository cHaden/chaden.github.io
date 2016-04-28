---
layout: post
title:  "Using 'rake stats' and 'rake notes'"
date:   2016-04-15 11:43:00 -0400
categories: ruby stuff
---

Two simple-but-useful `rake` tasks that I haven't seen mentioned a whole lot
in various "Learn Rails" tutorials:

`rake stats` and `rake notes`


---
---


`rake stats` can be used to get all sorts of information about your Rails app. Total lines of code (LOC), number of classes, number of methods, etc. It gives a quick rundown of just how big your app is getting, and draws attention to any ratios which might be getting a little crazy (say, lines of code per method)

![rake stats screenshot]({{site.url}}/assets/pics/rake_stats.jpg)


---
---


`rake notes` searches for comments starting with "FIXME", "OPTIMIZE" or "TODO." For any matches, it returns the filename, line number, and text of the comment.

To get comments with matching just one of these keywords, add the keyword to the end:
`rake notes:fixme`
