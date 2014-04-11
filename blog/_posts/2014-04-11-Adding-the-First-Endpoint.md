---
layout: post
title:  "Adding the First Endpoint to an API"
github: "https://github.com/jasontruluckblog/RAR-2-adding-first-endpoint"
date:   2014-04-11 00:00:00
categories: ruby rest api grape series testing rspec
series: "Building a REST API with Ruby"
series_url: "/blog/2014/04/04/Building-A-Rest-API.html"
---

Welcome back! For this part I am going to show you how we will add our first endpoint to the API. This will also introduce a few key concepts like
[mounting](https://github.com/intridea/grape#mounting) an API resource and [routing](https://github.com/intridea/grape#routes). I will also cover
adding our testing setup in this post, because of course we are doing TDD! But first, a quick background on what the API will do.

The API will be a very simple blogging/cms application, a venerable example :). When all is said and done, the API should have users,
administrators, posts, tagging, and more. But, for the first API endpoint we will add a way to check the status. This will be a super
simple endpoint that can be hit that will just return a `200 OK` response so someone using the API will be able to know if the API is reachable.

First things first, lets add [rspec](https://github.com/rspec/rspec) so we can start writing our test for this endpoint. You can of course exchange
these gems with ones of your choice like `test-unit`, but we will be using `rspec` throughout this series.

* Add `rspec`, `rack-test`, and `json_spec` to the `Gemfile`

<script src="http://gist-it.appspot.com/github.com/jasontruluckblog/RAR-2-adding-first-endpoint/blob/master/Gemfile?slice=5:"></script>

* Then we will add a `spec` folder to the project and create the `spec_helper.rb` file.

<script src="http://gist-it.appspot.com/github.com/jasontruluckblog/RAR-2-adding-first-endpoint/blob/master/spec/spec_helper.rb?"></script>

What is going on here is pretty straight forward, but heres a quick rundown: We are requiring our application.rb file, then creating an instance of our
API that tests will run against. The following block is a typical configuration block for `rspec` which you can read more about
[here](https://www.relishapp.com/rspec/rspec-core/v/3-0/docs/configuration).

* Next add the folder `api` under the `spec` folder and add a file `status_spec.rb`. This will serve as a space to place all of our specs for the API
endpoints.

<script src="http://gist-it.appspot.com/github.com/jasontruluckblog/RAR-2-adding-first-endpoint/blob/master/spec/api/status_spec.rb"></script>

If you are familiar with RSpec this should look pretty familiar. We are just using `rack-test` helpers to send a `get` request to the endpoint `/status`
then expecting a `200` response and the json `{ status: ok }`. This will of course return an error that the route can not be found since we have not yet
made it.

* Now that we have our spec all written up we can start working on the actual endpoint. First lets create the endpoint:

<script src="http://gist-it.appspot.com/github.com/jasontruluckblog/RAR-2-adding-first-endpoint/blob/master/app/api/status.rb"></script>

For this endpoint we are using a `GET` request and will not require any authentication. The `namespace` block will be the name of the resource
of our endpoint, `status`. The `desc` helper simply allows us to add a description to the endpoint, which can easily be used to generate documentation
(more on that soon) and overall adds clarity. The `get` block defines our request method and what should be run and returned, in this case simply JSON
returning `200 OK`.

If we run our specs now you will notice there is still an error about the missing route, this is because we still have to require the file and mount the
endpoint into the API. To do so we need to add to our `application.rb` and `boot.rb` files to include our new endpoint.

<script src="http://gist-it.appspot.com/github.com/jasontruluckblog/RAR-2-adding-first-endpoint/blob/master/config/boot.rb"></script>

<script src="http://gist-it.appspot.com/github.com/jasontruluckblog/RAR-2-adding-first-endpoint/blob/master/config/application.rb"></script>

You may notice that we now are using `ActiveSupport::Configurable`. This is a personal preference that I like for configuring common parts of the
application but it is not required. You could just as easily accomplish this using the previous way we instantiated `env` and just keep using
`File.expand_path` to create an equivalent `root`, whichever you prefer. Also, you will notice a method `include_files` which does just as you may
expect and is just an easy way to require additional files. In this case all of our public API files including `status.rb` that we have created.

And finally we mount the API endpoint into our API inside of the `app/api.rb` file.

<script src="http://gist-it.appspot.com/github.com/jasontruluckblog/RAR-2-adding-first-endpoint/blob/master/app/api.rb"></script>

Now when we re run the specs you should get a nice all green! Success, we have added our first endpoint! You can also now run `rackup` in your terminal
and go to `localhost:9292/status` to see your API in action. Don't forget to check out the source code for the whole project by clicking the Github
icon above!
