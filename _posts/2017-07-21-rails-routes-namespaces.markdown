---
layout: post
title:  "routing in Rails: namespaces"
date:   2017-07-21 11:43:00 -0400
categories: ruby stuff
---
Rails routing with namespaces -- it's weirder than you think!

{% highlight ruby %}
# if you have a controller in folder "group", declared like class Group::SomethingsController < ApplicationController
# your routes will look like:
#   Prefix Verb               URI Pattern                Controller#Action
#   group_items       POST   /group/items(.:format)      group/items#create
#   new_group_items   GET    /group/items/new(.:format)  group/items#new
#   edit_group_items  GET    /group/items/edit(.:format) group/items#edit
#                     GET    /group/items(.:format)      group/items#show
#                     PATCH  /group/items(.:format)      group/items#update
#                     PUT    /group/items(.:format)      group/items#update
#                     DELETE /group/items(.:format)      group/items#destroy
namespace :group do
  resource :items
end
{% endhighlight %}

{% highlight ruby %}
# if you have a controller in a namespace (in this example, "group"), but don't want the namespace in the
# helper methods or URI, use module:
# your routes will look like this:
  # Prefix Verb   URI Pattern           Controller#Action
  # items POST   /items(.:format)      group/items#create
  # new_items GET    /items/new(.:format)  group/items#new
  # edit_items GET    /items/edit(.:format) group/items#edit
  #    GET    /items(.:format)      group/items#show
  #    PATCH  /items(.:format)      group/items#update
  #    PUT    /items(.:format)      group/items#update
  #    DELETE /items(.:format)      group/items#destroy
resource :items, module: :group
{% endhighlight %}

{% highlight ruby %}
# another way to accomplish the same thing as above
# resource :items, controller: "group/items"

# if you want to arbitrarily choose a string to add to URIs that is not a namespace, use scope
# Prefix Verb   URI Pattern                       Controller#Action
# items POST   /not_a_group/items(.:format)      group/items#create
# new_items GET    /not_a_group/items/new(.:format)  group/items#new
# edit_items GET    /not_a_group/items/edit(.:format) group/items#edit
#    GET    /not_a_group/items(.:format)      group/items#show
#    PATCH  /not_a_group/items(.:format)      group/items#update
#    PUT    /not_a_group/items(.:format)      group/items#update
#    DELETE /not_a_group/items(.:format)      group/items#destroy
scope "not_a_group" do
  resource :items, module: :group
end
{% endhighlight %}

{% highlight ruby %}
# though, if you do your namespacing this way, you'll have both the name of the namespace and your arbitrary scope string in the URI
# Prefix Verb   URI Pattern                                 Controller#Action
# group_items GET    /not_a_group/group/items(.:format)          group/items#index
#        POST   /not_a_group/group/items(.:format)          group/items#create
# new_group_item GET    /not_a_group/group/items/new(.:format)      group/items#new
# edit_group_item GET    /not_a_group/group/items/:id/edit(.:format) group/items#edit
# group_item GET    /not_a_group/group/items/:id(.:format)      group/items#show
#      PATCH  /not_a_group/group/items/:id(.:format)      group/items#update
#      PUT    /not_a_group/group/items/:id(.:format)      group/items#update
#      DELETE /not_a_group/group/items/:id(.:format)      group/items#destroy
scope "not_a_group" do
  namespace :group do
    resources :items
  end
end
{% endhighlight %}

{% highlight ruby %}
# ... and, if you flip your nesting, you'll see the names flipped in order in the URI
#   Prefix Verb   URI Pattern                                 Controller#Action
# group_items GET    /group/not_a_group/items(.:format)          group/items#index
#        POST   /group/not_a_group/items(.:format)          group/items#create
# new_group_item GET    /group/not_a_group/items/new(.:format)      group/items#new
# edit_group_item GET    /group/not_a_group/items/:id/edit(.:format) group/items#edit
# group_item GET    /group/not_a_group/items/:id(.:format)      group/items#show
#        PATCH  /group/not_a_group/items/:id(.:format)      group/items#update
#        PUT    /group/not_a_group/items/:id(.:format)      group/items#update
#        DELETE /group/not_a_group/items/:id(.:format)      group/items#destroy
namespace :group do
  scope "not_a_group" do
    resources :items
  end
end
{% endhighlight %}

{% highlight ruby %}
# Of course, there's nothing forcing you to make your scope name arbitrary. You could give it the same name
# as your namespace, and have the namespace show up in the URI, but not the helper methods (Why do you want that? I don't know. But you could!)
# Prefix Verb   URI Pattern                 Controller#Action
#  items POST   /group/items(.:format)      group/items#create
# new_items GET    /group/items/new(.:format)  group/items#new
# edit_items GET    /group/items/edit(.:format) group/items#edit
#        GET    /group/items(.:format)      group/items#show
#        PATCH  /group/items(.:format)      group/items#update
#        PUT    /group/items(.:format)      group/items#update
#        DELETE /group/items(.:format)      group/items#destroy
scope "group" do
  resource :items, module: :group
end
{% endhighlight %}
