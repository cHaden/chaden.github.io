---
layout: post
title:  "Using procs in case statements whaaaaaa?"
date:   2016-05-02 11:43:00 -0400
categories: ruby stuff
---



So, we've all seen a basic `case` statement in Ruby, where you
do different things based on a variable having different values:
{% highlight ruby %}
def basic_case(some_number)
  case some_number
  when 1
    "one"
  when 2
    "that's a two"
  else
    "pfft. other numbers"
  end
end
{% endhighlight %}

...but, what if you want to switch based on a more involved set of rules? Maybe,
if the value is odd, or falls within a range of values. Did you know what you can just
throw a proc in there?

{% highlight ruby %}
def head_splode(input)
  case input
  when ->(s) { s.odd? }
    "odd"
  when ->(s) { s > 45 && s < 100 }
    "arbitrary even number"
  else
    "I dunno"
  end
end
{% endhighlight %}


If you want to get really fancy, you can even pull those procs out into methods,
making your case statement easier to read:

{% highlight ruby %}
def input_is_odd?
  ->(s) { s.odd? }
end

def between_range?
  ->(s) { s > 45 && s < 100 }
end

def head_splode(input)
  case input
  when input_is_odd?
    "odd"
  when between_range?
    "arbitrary even number"
  else
    "I dunno"
  end
end
{% endhighlight %}

Please note that this can get somewhat confusing, as any case assumes that none of the
previous cases applied. Don't overdo it with overly complex case statements!
