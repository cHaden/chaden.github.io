---
layout: post
title:  "Using 'module_function' and 'extend self'"
date:   2016-04-12 11:43:00 -0400
categories: ruby stuff
---

So, say that you have a module, and you want to use a method from it as both an
instance method in a class, but also as a module function.

For example, given a module and class like so:
{% highlight ruby %}
module SomeFunctions
  def i_do_a_thing
    puts "doing stuff"
  end
end

class ThingDoer
  include SomeFunctions
end
{% endhighlight %}

Attempting to call `i_do_a_thing` as an instance method of `ThingDoer` works, but
you can't call it as a module function:
{% highlight ruby %}
#this works as expected:
td = ThingDoer.new
td.i_do_a_thing

#...but this doesn't. You get an error "undefined method `i_do_a_thing' for SomeFunctions:Module (NoMethodError)"
SomeFunctions.i_do_a_thing
{% endhighlight %}

Your first response might be to change the module method:
{% highlight ruby %}
module SomeFunctions
  def self.i_do_a_thing
    puts "doing stuff"
  end
end
{% endhighlight %}

...but this means that the method can no longer be called as an instance method of `ThingDoer`
{% highlight ruby %}
#now this works:
SomeFunctions.i_do_a_thing

#but this doesn't. You're back to getting an error, "undefined method `i_do_a_thing' for #<ThingDoer:0x007fee9b80f980> (NoMethodError)"
td = ThingDoer.new
td.i_do_a_thing
{% endhighlight %}

Using `module_function` makes the specified method available as a private method in class `ThingDoer`,

{% highlight ruby %}
module SomeFunctions
  def i_do_a_thing
    puts "doing stuff"
  end
  module_function :i_do_a_thing
end
{% endhighlight %}

...although this introduces some limitations:
{% highlight ruby %}
#this still doesn't work, because i_do_a_thing is treated as a private method
td = ThingDoer.new
td.i_do_a_thing

#this works just fine
SomeFunctions.i_do_a_thing
{% endhighlight %}

If desired, `i_do_a_thing` can be made available as a public method by adding a public method to `ThingDoer`
{% highlight ruby %}
class ThingDoer
  include SomeFunctions

  def do_the_thing
    i_do_a_thing
  end
end
{% endhighlight %}

Now the same functionality of `i_do_a_thing` is available as a public instance method
{% highlight ruby %}
#this works just fine
td = ThingDoer.new
td.do_the_thing

#this works just fine
SomeFunctions.i_do_a_thing
{% endhighlight %}

Note that you can get very similar results with use of `extend self` in lieu of `module_function`
{% highlight ruby %}
module SomeFunctions
  extend self
  def i_do_a_thing
    puts "doing stuff"
  end
end
{% endhighlight %}

Calls can be made in the same way as we did with `module_function`
{% highlight ruby %}
#'extend self' will achieve similar results. This works:
td = ThingDoer.new
td.do_the_thing

#...and this works too
SomeFunctions.i_do_a_thing
{% endhighlight %}

One difference with `extend self` is that methods from the module will be available as public instance methods in the class that includes the module:

{% highlight ruby %}
#methods from your included module are not private
td = ThingDoer.new
td.i_do_a_thing
{% endhighlight %}


___


Additional Reading:

[Ruby documentation][documentation]

[A helpful blog post][blog-post]


[documentation]: http://apidock.com/ruby/v1_9_3_392/Module/module_function
[blog-post]: http://idiosyncratic-ruby.com/8-self-improvement.html
