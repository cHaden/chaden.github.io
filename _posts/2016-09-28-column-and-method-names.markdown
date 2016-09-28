---
layout: post
title:  "methods with the same name as columns in ActiveRecord objects"
date:   2016-09-28 11:43:00 -0400
categories: ruby stuff
---

When working with an ActiveRecord object, what happens if you happen to define
a method with the same name as a column in your schema?

My assumption was that the method name would always override, but things are a
bit more counter-intuitive.

For example, take an ActiveRecord object `Thing` with a schema like this:
{% highlight ruby %}
create_table "things", force: :cascade do |t|
  t.string   "name"
  t.integer  "points"
  t.datetime "created_at", null: false
  t.datetime "updated_at", null: false
end
{% endhighlight %}

and a class definition like this:

{% highlight ruby %}
class Thing < ActiveRecord::Base
  def name
    "not my name"
  end
end
{% endhighlight %}

Normally, you might access `name` on a `Thing` like:

{% highlight ruby %}
some_thing.name
{% endhighlight %}

but, in this case, instead of getting what was stored in the database, you would
always get the string "not my name"

However, this also allows some weird things.

For example:

{% highlight ruby %}
# let's say that this was what was already in the database:
#  => #<Thing id: 3, name: "something something", points: 4, created_at: "2016-09-28 18:37:24", updated_at: "2016-09-28 18:42:44">

some_thing.name = some_thing.name #this seems like a tautology, and yet...
some_thing.save

# now it looks like this:
#  => #<Thing id: 3, name: "not my name", points: 4, created_at: "2016-09-28 18:37:24", updated_at: "2016-09-28 18:42:44">
{% endhighlight %}
