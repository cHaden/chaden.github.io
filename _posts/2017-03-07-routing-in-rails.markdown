---
layout: post
title:  "routing in Rails"
date:   2017-03-07 11:43:00 -0400
categories: ruby stuff
---

Let's learn about routes!

{% highlight ruby %}
  # => You can have the root of your site routed with "root"
  # => format '<controller_name>#<controller_method_name>'
  # => this is the format that shows up in the rightmost column when
  # => you run 'rake routes'
  root 'posts#index'
{% endhighlight %}

{% highlight ruby %}
  # => don't even try it like this:
  root '/posts' #this craps out
  root 'posts' #this craps out
  root posts #this craps out
{% endhighlight %}

{% highlight ruby %}
  # => this is the default way in which resources is declared when
  # => doing a scaffold
  resources :posts

  # => these work too:
  resources 'posts'
  resources "posts"

  # => this craps out. No slashes!
  # => (the error is "'/posts' is not a supported controller name")
  resources "/posts"
{% endhighlight %}

{% highlight ruby %}
  # => if you don't want to create all seven RESTful routes,
  # => you can explicitly state which ones you want using "only"
  resources "posts", :only => [:index, :create, :show]

  # => you can also do the reverse, and explictly state which RESTful
  # => routes to leave out
  resources "posts", :except => :destroy
{% endhighlight %}

{% highlight ruby %}
  # => if you add something which is not one of the standard RESTful routes,
  # => it just gets ignored (whether a method of that name exists in the
  # => controller or not)
  # => for example, "bumblesnoot" does nothing even if you have a method
  # => named "bumblesnoot"
  resources "posts", :only => [:index,
                               :create,
                               :show,
                               :do_nothing_much,
                               :bumblesnoot]
{% endhighlight %}

{% highlight ruby %}
  # => now, this is a little weird, and is part of why pluralization in
  # => Rails is mind-bending
  # => using "resource" instead of "resources" still creates your seven
  # => RESTful routes, but in the singular.
  # => by default, index is left out, but can be explicitly added with :only
  # => "resource 'posts'" refers to the same controller as if you did
  # => "resources 'posts'"", but the URI and prefixes
  # => are a little bit different. (It's used to indicate that only one
  # => instance of this will exist)
  # => you get routes like this with the singular 'resource':
  # => Prefix    Verb   URI Pattern          Controller#Action
  # => post      POST     /post(.:format)    posts#create
  # => new_post  GET    /post/new(.:format)  posts#new
  # => edit_post GET    /post/edit(.:format) posts#edit
  # =>           GET    /post(.:format)      posts#show
  # =>           PATCH  /post(.:format)      posts#update
  # =>           PUT    /post(.:format)      posts#update
  # =>           DELETE /post(.:format)      posts#destroy
  resource "post"
{% endhighlight %}

{% highlight ruby %}
  # => if you want to get crazy, you can have a "resource" and a
  # => "resources" statement for the same controller in the same project
  # => (You're setting yourself up for confusion, but you can do it)
  resource "post"
  resources "post"
{% endhighlight %}

{% highlight ruby %}
  # => to add a route from the controller, beyond the seven default RESTful
  # => routes, you can use "member"
  # => BUT the URI requires an id, even if this method has
  # => nothing to do with a specific instance
  resources :posts do
    member do
      get 'do_nothing_much'
      # this would be accessible via /posts/:id/do_nothing_much

      post 'do_nothing_much'
      # access via post: this is similar to the line
      # above, only note that you can't directly type in a URL like
      # "http://somedomain/posts/3/do_nothing_much" and have it work

      get "bob", action: "do_nothing_much"
      # your route doesn't have to have the same name as the controller
      # method that it calls this would be accessible like
      # bob_post GET    /posts/:id/bob(.:format)  posts#do_nothing_much

      get ":bob", action: "do_nothing_much", as: "bob"
      # your route can take another variable you could access this like
      # bob_post GET    /posts/:id/:bob(.:format)   posts#do_nothing_much
      # (Or, a URL like http://somedomain/posts/123/456)
     end
   end
{% endhighlight %}

{% highlight ruby %}
  # => "collection" is similar to 'member', only now,
  # => the URI does not require an id
  resources :posts do
    collection do
      get 'do_nothing_much'
      # you can access this via "http://somedomain/posts/do_nothing_much"

      post 'do_nothing_much'
      # again, you can't type in a URL like the above for post
    end
  end
{% endhighlight %}

{% highlight ruby %}
  # => this is another way to do the same thing as above
  resources :posts do
    get 'do_nothing_much', on: :collection
  end
{% endhighlight %}

{% highlight ruby %}
  # => this is yet another way to do the same thing as above,
  # => EXCEPT it allows renaming to something other than the method name.
  # => so, instead of getting this route info:
  # => do_nothing_much_posts GET    /posts/do_nothing_much(.:format) posts#do_nothing_much
  # => you get this:
  # => foo_posts GET    /posts/foo(.:format)      posts#do_nothing_much
  resources :posts do
    get 'foo' => 'posts#do_nothing_much', on: :collection
  end
{% endhighlight %}

{% highlight ruby %}
  # => If you want to create your resources with an arbitrary string for
  # => the prefix and URI
  # => You can specify which controller to associate these routes with like so:
  resources "not_a_post", :controller => "posts"
  # => This gives you routes like
  # =>           Prefix Verb   URI Pattern                    Controller#Action
  # => not_a_post_index GET    /not_a_post(.:format)          posts#index
  # =>                  POST   /not_a_post(.:format)          posts#create
  # =>   new_not_a_post GET    /not_a_post/new(.:format)      posts#new
  # =>  edit_not_a_post GET    /not_a_post/:id/edit(.:format) posts#edit
  # =>       not_a_post GET    /not_a_post/:id(.:format)      posts#show
  # =>                  PATCH  /not_a_post/:id(.:format)      posts#update
  # =>                  PUT    /not_a_post/:id(.:format)      posts#update
  # =>                  DELETE /not_a_post/:id(.:format)      posts#destroy
{% endhighlight %}

{% highlight ruby %}
  # => Example of regular, specific route:
    get 'products/:id' => 'catalog#view'
  # => note: if 'resources' has been done for the given controller, make
  # => sure your explicit get/post/etc statements are nested inside.
  # => Otherwise, weirdness happens

  # => the first URI param can be more or less anything
  # => the second param is in the format '<controller name>#<method name>'
  get 'posts/do_nothing_much' => 'posts#do_nothing_much' # can be accessed at http://somedomain/posts/do_nothing_much
{% endhighlight %}

{% highlight ruby %}
  # => this does the same thing as above
  get 'posts/do_nothing_much', :to => 'posts#do_nothing_much'
{% endhighlight %}

{% highlight ruby %}
  # => the first parameter does not need to reflect where the method
  # => lives in code
  get 'do_nothing_much' => 'posts#do_nothing_much'
  # can be accessed at http://somedomain/do_nothing_much
{% endhighlight %}

{% highlight ruby %}
  # => the preceeding '/' doesn't matter. Put in a bunch if you're feeling crazy.
  # => I mean, don't, though. It's super confusing.
  get '/posts/do_nothing_much' => 'posts#do_nothing_much' # can be accessed at http://somedomain/posts/do_nothing_much
  get '/////arbitrary/strings/dont/matter/do_nothing_much' => 'posts#do_nothing_much'
  # can be accessed at http://somedomain/arbitrary/strings/dont/matter/do_nothing_much
{% endhighlight %}

{% highlight ruby %}
  # => spaces are a PITA, although they will technically work, just with
  # => a '%20' in there
  get 'not a failure state' => 'posts#do_nothing_much'
  # can be accessed at http://somedomain/not%20a%20failure%20state
{% endhighlight %}

{% highlight ruby %}
  # => special characters are terrible. Don't do this just because you can
  get 'éllo failure' => 'posts#do_nothing_much'
  # can be accessed at http://somedomain/%C3%A9llo%20failure
{% endhighlight %}

{% highlight ruby %}
  # => this is fine, but as post, it is not accessible by just
  # => typing in http://somedomain/do_nothing_much
  post 'do_nothing_much' => 'posts#do_nothing_much'
{% endhighlight %}

{% highlight ruby %}
  # => another way to do a get. This isn't a great practice, because
  # => hard coding URLs is a problem if said URLS ever change
  get 'foo' => redirect( 'http://somedomain' )
{% endhighlight %}

{% highlight ruby %}
  # => this is another way to do redirects
  get 'foo' => redirect( '/posts')

  # => this does the same thing
  get 'foo' => redirect( 'posts')
{% endhighlight %}

{% highlight ruby %}
  # => adding an 'as:' means that your routes go from something like
  # =>  foo GET  /foo(.:format) posts#do_nothing_much
  # => to something like
  # =>  bar GET  /foo(.:format) posts#do_nothing_much
  # => this is a little bit subtle. You still access like http://somedomain/foo
  # => BUT, in code, you can use the prefix 'bar', for helpers like 'bar_url'
  get 'foo' => 'posts#do_nothing_much', as: :bar
{% endhighlight %}

{% highlight ruby %}
  # => Nested resources: this creates your standard RESTful routes for both
  # => the Users and Posts controller,
  # => but associates posts more closely with users.
  # => Example: this creates a route like
  # =>    new_user_post GET  /users/:user_id/posts/new(.:format)   posts#new
  # => instead of the
  # =>    new_post GET    /posts/new(.:format)      posts#new
  # => you would get without nesting
  resources :users do
    resources :posts
  end
{% endhighlight %}

{% highlight ruby %}
  # => if you have overlapping routes, the first one declared is the one used
  # => example: here, the helper "users_path" would go to the create method
  # => of the Users controller
  get "show" => "users#create"
  get "show" => "users#show"
  # => ...while here, "users_path" would go to the show method
  get "show" => "users#show"
  get "show" => "users#create"
{% endhighlight %}

{% highlight ruby %}
  # => note: 'concerns' are a Rails 4 thing, I believe
  # => This is useful if you have a method that you wish to associate with
  # => more than one controller
  # => first you declare your concern like this:
  concern :yourmom do
    get 'do_nothing_much'
  end
  # => and then use it in the following format
  resources :posts, concerns: :yourmom
  # this looks for the "do_nothing_much" method in the posts controller

  resources :users, concerns: :yourmom
  # this looks for the "do_nothing_much" method in the users controller

  # => note: when declared like this, you might stumble, because it only finds
  # => the method called 'do_nothing_much' in whatever controller you're
  # => talking about when you use 'concerns:'
{% endhighlight %}

{% highlight ruby %}
  # => to always use the 'do_nothing_much' method from a *specific*
  # => controller, declare like this:
  concern :yourmom do
    get 'do_nothing_much' => 'posts#do_nothing_much'
  end
  resources :posts, concerns: :yourmom
  resources :users, concerns: :yourmom
{% endhighlight %}

{% highlight ruby %}
  # => this is similar to nesting resources, except the namespace can be an
  # => arbitrary name, and not reference any particular controller name
  namespace :foo do
    resources :posts
  end
{% endhighlight %}

{% highlight ruby %}
  # => match is similar to "get" or "post", except that it declare both verbs
  # => at the same time
  # => note: this will not work for Rails 4 or above
  match "do_nothing_much" => "posts#do_nothing_much"
  # => for versions of Rails after 3, the request method needs to be
  # => explicitly stated. It might be a good idea to avoid using match
  # => altogether, it seems to be falling out of favor
  match "do_nothing_much" => "posts#do_nothing_much", :via => [:get, :post]
  # => you can also use :as with match statements as you would with
  # => get statements
  match "do_nothing_much" => "posts#do_nothing_much",
        :via => :get,
        :as => "this_only_changes_the_helper_name"
{% endhighlight %}
