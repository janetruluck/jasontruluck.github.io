---
layout: post
title:  "Code Writing Code"
github: "https://github.com/jasontruluck/resource-generators"
date:   2014-04-23 00:00:00
categories: ruby thor automation generators
---
<br/>

### Dont write code! Write code to write your code!

Okay, funny meta statements aside, I noticed this week that in a side project I was doing a whooole lot of copy and 
pasting. To me this was a clear sign I could, and should, try to automate what I was doing. Clearly copy and pasting 
is pretty error prone and has caused me enough fuss that I try to avoid it when possible. As much as I would like 
to believe I am as consistent as a computer, unfortunately, I am not... Enter in [Thor](https://github.com/erikhuda/thor)! There may be something better
out there for this task but given I wanted to whip up something quick and always wanted to give Thor a go I figured 
why not. After all, if its good enough for Rails I am sure its good enough for me :).

A little background on the task at hand. The project is only a simple Rack API (which is why I am not extending
or creating a Rails generator). When I am making a resource, for example `Companies`, I have have a good few files in common
that in turn share a good bit of general functionality. Those files in the case of a `Company` are:

* `company.rb` (Model)
  * Creating a [Grape::Entity](https://github.com/intridea/grape-entity)
  * Setting up accessible attributes for both public and admin use
  * validations
  * relationships
  * scopes
  * extras like versioning and ordering
* Migration for the `company` table
  * Setting uuids and general attributes
* `company_version.rb` (Version class for use with the [PaperTrail](https://github.com/airblade/paper_trail) gem)

* Migration for the `company_version` table

* `company_spec.rb` (RSpec Model Spec) 
  * attributes specs
  * validation specs
  * relationship specs
* `company_factory.rb` (Factory Girl Factory for the resource) 
  * Nice field definitions based on the attribute type

This begins to look a lot like a scaffolding in Rails, so it made sense to use a `Thor::Group`, which will run through
the `Thor::Group` sequentially running each method. Perfect! Just the thing I was looking for, why bother 
manually creating these files and filling it with a bunch of repeated boilerplate that I know I will need? 

Thor makes it easy to generate files to your preference using familiar `erb` templates. This was both really cool and 
a point of pain for me. It was really awesome to have a common syntax to write these templates in, but holy crap was it
ugly looking. A lot of the things I had to do, for example formatting line indentation, was really hacky and removed a 
good bit of nice formatting from the files in order to preserve nice formatting in the generated files. Take these criticisms
with a grain of salt since there very well may be a nicer templating system for use in Thor or just a better way in general 
(if so leave me a comment about it!) I admittedly only did a quick Google search for an alternative to no avail. It would
be really cool to see something similar to handlebars/mustache or jinja2 style adapted for use. 

Overall the experience was very enjoyable and help make development faster and more consistent, exactly the goal hooray!
Take a look at the [code](https://github.com/jasontruluck/resource-generators) for all the details and feel free to ask questions in the comments below! 
