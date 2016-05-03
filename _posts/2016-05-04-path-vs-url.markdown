---
layout: post
title:  "_path vs _url in Rails routes"
date:   2016-05-04 11:43:00 -0400
categories: ruby stuff
---

If, for example, your routes file includes the line `resources :users`, and
you wish to have a URI to the index view for Users, you might use `users_path` or `users_url`
in your code.

...but which one is right? Is there a difference?

The short answer: `_path` creates a relative url, and `_url` produces an absolute url.

The commonly accepted answer seems to be "use absolute urls for redirects, or
when linking between an SSL and non SSL site, and use relative urls for everything
else"

Some sources suggest that absolute urls are preferred for SEO reasons, however,
this appears to be out of date information, if it was ever true. The consensus seems
to be that modern search engine crawlers can figure out relative urls. In fact, some
  sources suggest that relative urls can be now handled for redirects too.

#### Resources:
[Rails Named Routes: Path Vs. URL][path-vs-url]

[Should I Change My Relative Links to Absolute Links for SEO Purposes?][relative-urls]

[path-vs-url]: https://www.viget.com/articles/rails-named-routes-path-vs-url
[relative-urls]: https://www.squareroot-inc.com/should-i-change-my-relative-links-to-absolute-links-for-seo-purposes/
