---
layout: post
title:  "Building a REST API with Ruby"
github: "https://github.com/jasontruluckblog/building-a-rest-api"
date:   2014-04-04 00:00:00
categories: ruby rest api grape series
---

Over the next couple of weeks I want to go over how to build a RESTful API with Ruby. I plan to cover a
bunch of different topics in a "plug and play" kind of style something from the very, very basics of a
simple API skeleton to things like adding persistance, authentication, performance, testing, and other awesome goodies.
If you have something you would like me to cover feel free to leave me a suggestion in the comments or open an issue on
Github!

Like many things in programming choosing how to approach problems as broad as this offer several options.
For this series I chose to use the following technologies where applicable. This list will surely expand as more
topics are covered:

#### Base
Core items that we will build the API off of

* [Rack](https://github.com/rack/rack) as the base to build off of for handling all of our requests (this is an API after all...)
* [Grape](https://github.com/intridea/grape) as the DSL for getting out API up and running. Grape offers a ton
of niceties out of the box and given its domain is perfect for this use case. Plus, in my opinion its just awesome.

#### Persistance
Items we will use to build a persistance layer off of for saving data, what good would an API be without it?!

* [Active Record](https://github.com/rails/rails/tree/master/activerecord) for the ORM to interact with the database. Given this is
an API I can understand objections to this for performance reasons, but in my opinion the convention and safety Active Record brings
you is a great trade off (initially) for speed. Worst comes to worse, you can always drop down and muck in SQL if you want to, but to do
so off the bat is a bit of premature optimization in my eyes. Iterate, iterate, iterate...
* [PostgresSQL](http://www.postgresql.org/) for the database. Postgres is a great relational database that has a ton of awesome extensions
and great performance. Most everything covered through this series should be interchangeable with your database of choice, but be aware it
is build with Postgres in mind.

#### Representation
The API will primarily return JSON. These are items used to represent resources in our API and to make them all gussied up for public
consumption.

* [Grape Entity](https://github.com/intridea/grape-entity) given we are using Grape, it makes sense to use their gem for representation.
It is super simple to use, offers a lot of power, and sits right on top of the model, which I really like.

#### Testing
Testing the API is super important (just like any other app!) and will help maintain the API as it grows.

* [rspec](https://github.com/rspec/rspec) an awesome BDD too for writing the specs.
* [json_spec](https://github.com/collectiveidea/json_spec) for easy assertions against the JSON the API will be returning
* [factory_girl](https://github.com/thoughtbot/factory_girl) for easy object creation during tests.
* [database_cleaner](https://github.com/bmabey/database_cleaner) for cleaning up the DB during testing.
* [shoulda-matchers](https://github.com/thoughtbot/shoulda-matchers) for making our tests pretty on the eyes.

### Contents
* [About](/blog/2014/04/04/Building-A-Rest-API.html)
* Setup
  * [Adding Grape](/blog/2014/04/03/Adding-Grape-to-a-Rack-Application.html)