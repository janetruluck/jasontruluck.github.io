---
layout: post
title:  "Easily Disable ActiveRecord SQL Logging Conditionally"
date:   2013-08-06 15:48:40
categories: ruby rails logging sql postgresql
---
Sometimes reading through all the logging ActiveRecord does  when running rails server and console can be a pain. 
It can muddle things up and make it pretty hard to debug an issue you may be having. This is a super easy trick to 
subjectively disable logging for your rails server or rails console. The main way of going about this is:

{% highlight ruby %}
  # config/environment.rb
  ActiveRecord::Base.logger = Logger.new('/dev/null')
{% endhighlight %}

What this does is dump the logging from ActiveRecord into a null buffer never to
be seen again. Yay! This is great but now everytime you want to allow sql
logging you need to go back in and remove this line or comment it out. This is
brittle and can lead to confusion if you or a coworker forgets you slapped this
in your environment file. So the simple way around this is to wrap it in an
environment  variable like so:

{% highlight ruby %}
  # config/environment.rb
  if ENV["NOSQL"]
    ActiveRecord::Base.logger = Logger.new('/dev/null')
  end
{% endhighlight %}

Now whenever you want to disable SQL logging simply call:
{% highlight ruby %}
  NOSQL=true rails s
{% endhighlight %}

and if you do want to have SQL logging just leave it off like normal and you are
good to go:
{% highlight ruby %}
  rails s
{% endhighlight %}

This will also work nice with foreman like so:
{% highlight ruby %}
  NOSQL=true foreman start
{% endhighlight %}

Like I said, super easy right?! 
