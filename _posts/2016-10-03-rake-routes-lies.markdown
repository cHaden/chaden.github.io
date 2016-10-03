---
layout: post
title:  "rake routes is only so accurate..."
date:   2016-10-03 11:43:00 -0400
categories: ruby stuff
---

A tiny lightbulb has turned on for me: `rake routes` _only_ looks at the routes.rb
file.

For example, if you have a line like:

{% highlight ruby %}
resources :notacontroller
{% endhighlight %}

in your routes files, `rake routes` will blithely tell you that you have all sorts
of routes available

{% highlight ruby %}
notacontroller_index GET    /notacontroller(.:format)          notacontroller#index
                     POST   /notacontroller(.:format)          notacontroller#create
  new_notacontroller GET    /notacontroller/new(.:format)      notacontroller#new
 edit_notacontroller GET    /notacontroller/:id/edit(.:format) notacontroller#edit
      notacontroller GET    /notacontroller/:id(.:format)      notacontroller#show
                     PATCH  /notacontroller/:id(.:format)      notacontroller#update
                     PUT    /notacontroller/:id(.:format)      notacontroller#update
                     DELETE /notacontroller/:id(.:format)      notacontroller#destroy
{% endhighlight %}

...whether that particular controller exists or not.

Be mindful when trying to troubleshoot Rails routing issues. It is treacherous
territory.
