---
layout: post
title:  "Adding Grape to a Rack Application"
github: "https://github.com/jasontruluckblog/RAG-1-adding-grape"
date:   2014-04-04 00:00:01
categories: ruby rest api grape series
series: "Building a REST API with Ruby"
series_url: "/blog/2014/04/04/Building-A-Rest-API.html"
---

So first will we start off with a simple Rack application and then add Grape on top.
It will have a pretty familiar structure if you have used Rails or another Ruby framework:

<pre>
  .
  +-- app
  |   +-- api
  |   |   +-- files for the api...
  |   +-- api.rb
  +-- config
  |   +-- environments
  |   |   +-- development.rb
  |   |   +-- test.rb
  |   +-- initalizers
  |   |   +-- files for the initialization...
  |   +-- application.rb
  |   +-- boot.rb
  |   +-- environment.rb
  +-- lib
  +-- config.ru
  +-- Gemfile
  +-- Rakefile
</pre>

Adding [Grape](https://github.com/intridea/grape) on top of this app is super simple. All you have to do is:

1.Add [Grape](https://github.com/intridea/grape) to your gemfile

<script src="http://gist-it.appspot.com/github.com/jasontruluckblog/RAG-1-adding-grape/blob/master/Gemfile?footer=minimal"></script>

2.Create the main api file that will mount the API endpoints and work to initialize the API.

<script src="http://gist-it.appspot.com/github.com/jasontruluckblog/RAG-1-adding-grape/blob/master/app/api.rb?footer=minimal"></script>

This will declare the format that our API will respond with, JSON, as well as setup a catchall route
for any path not declared in our mounted API files.

3.Require the API main api file into the application

<script src="http://gist-it.appspot.com/github.com/jasontruluckblog/RAG-1-adding-grape/blob/master/config/application.rb?footer=minimal&slice=12"></script>

4.Add the api to the `config.ru` so it will be run when you use `rackup`

<script src="http://gist-it.appspot.com/github/jasontruluckblog/RAG-1-adding-grape/blob/master/config.ru?footer=minimal"></script>

And thats pretty much all there is to it! Stay tuned, in the next post I will discuss adding the first endpoint!