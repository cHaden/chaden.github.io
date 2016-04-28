---
layout: post
title:  "Custom rake tasks"
date:   2016-04-25 11:43:00 -0400
categories: ruby stuff
---

Things learned:

* use `rake -T` to see available rake tasks

* Use `rake db:migrate VERSION=x` where x is the timestamp of the specific migration you wish
to perform (the number at the start of the filename). (Use caution.)

* Make new rake tasks in /lib/tasks.
   Generally follow the format of:   

  {% highlight ruby %}
  namespace :foo do
    task :bar do
      #do a thing
    end
  end
  {% endhighlight %}

   which can be called in the command line like `rake foo:bar`          

* Note that this doesn't do Rails'y stuff out of the box (like, say, ActiveRecord transactions)

   Working with ActiveRecord within a rake task might look more like this:

  {% highlight ruby %}
  namespace :foo do
    task :bar => :environment do
      ActiveRecord::Base.transaction do
        ActiveRecordObject.create( content: "blah blah blah" )
      end
    end
  end
  {% endhighlight %}
