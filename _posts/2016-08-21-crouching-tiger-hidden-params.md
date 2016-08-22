---
layout: post
section-type: post
title: Crouching Tiger, Hidden 'Params'
category: tech
tags: [ 'tutorial' ]
---
<img align="center" src="https://s-media-cache-ak0.pinimg.com/736x/fb/fd/01/fbfd0190689ae2b000da617d67ac2f75.jpg" alt="...">

Well, maybe they're not so hidden.

_**Disclaimer:** At the begining of the week, I went full naive with Rails. Never go FULL NAIVE. I thought Rails would make me a full blown wizard with Ruby. That, I would simply throw around some key commands, macros, and methods to help me build out my applications. Believing that Rails is magic is what will destroy me. Instead, I've re-acclimated myself with one of my learner principles: understand it to gain power over it, or don't and be enslaved to its mystery._

### What are Params?

Params or Parameters, at their core, in Rails framework are nothing more than a hash of key-value pairs. These key-value pairs can be used for anything from establishing website paths to database inputs/outputs.

Params allow for a more dynamic application. It reduces the amount of static pages or views needed to be created in order to show users what they're looking for when they make their web requests.

Let's use a case study with food. When a user makes this web request:

<pre><code>
https://www.example.com/foods/1
</code></pre>

We can read this as:

  * **Domain:** example.com
  * **Path:** /foods/1 
    * _#-> the last part "/1" being the parameters or params_
  * **Parameters:** /1


The domain is the website and the path is where on the website the user wishes to go and view. At the end of the path, the user has inputted a parameter. If our Rails application controller has established a viable route, then the user should be able to see a particular item within all "foods" index.

In the Rails application, specifically "/config/routes.rb", we should have established:

<pre><code class="ruby">
Rails.application.routes.draw do
    get '/foods/:id', to: 'foods#show'

end
</code></pre>

The above route enables a user to make a get request like 'http://www.example.com/foods/1'. Notice the "/:id" part of the route. That's the parameter we're establishing for our application and the user experience. The 'foods#show' part of that line is telling the route where to go when its being requested. When the user makes that specific web request, it'll lead to the "foods" controller, and therein should be the "show" method:

<pre><code class="ruby">
class FoodsController < ApplicationController

    def show
        @food = Food.find(params[:id])
    end

end
</code></pre>

And here is the magic! Or..

<img align="center" src="http://s2.quickmeme.com/img/1e/1e9494df0b206c608e42ca07e6746e16691b2ccc7e573c32234ee5bb427d7803.jpg" alt="...">

But, as soon as the initial shock and awe of Rails magic subsided, I realized:

<img align="center" src="http://www.openminds.tv/wp-content/uploads/Degrasse-not-aliens.jpg" alt="...">

### How do Params work?

Params is merely recording the user's get request "input value" that corresponds to the key-value pair we've established. In our database, and subsequently our index page, we have a list of 'foods' - each with a unique id or instance. The user is querying for the first one, with the id of 1. Or { "id" => "1" }. When we created our routes, we were subsequently creating a key of ":id". Params as a method inherited in ApplicationController which also inherits from ActionController::Base helps us record hashes to be used in our controller actions.

Here's what I imagine it would look like as a method:

<pre><code class="ruby">
class Parameter
    attr_reader :key_value
    
    def initialize(params={})
        @params = params
    end

end
</code></pre>

In the above, a new instance of a parameter would initialize with an empty hash. As the application creator, when I set my get route '/foods/:id', the ':id' is now being set as a key for my params. This signifies to my controller that any input in the '/:id' space will be the value to that "id" key.

<pre><code class="ruby">
https://www.example.com/foods/1

params = { :id => "1"}
</code></pre>

So, it's not really magic. Params is taking in and also recording parameters as set by the application and called on by the user.

### How else does params help me?

Knowing about params and setting them in the controller actions isn't enough. Params can be a liability because of it's hash-ful nature. An end-user can modify the html code in their browser to work through the hash and return or submit to the database adverse to the owner's intent. For instance, on an edit form, I may only include the field for a user to edit his/her own ':name' of the food, but he/she may be able to edite the field to be able to edit the ':items' of the food, when I didn't explicit want him/her to. Take a look:

<pre><code class="ruby">
# app/views/foods/edit.html.erb

<%= form_for(@food) do |f| %>
<%= f.text_field :name %>
<%= f.submit %>
</code></pre>

If I left my controller as is with the following added 'edit' and 'update' controller actions:

<pre><code class="ruby">
class FoodsController < ApplicationController

    def show
        @food = Food.find(params[:id])
    end

    def edit
        @food = Food.find(params[:id])
    end

    def update
        food = Food.find(params[:id])
        food.update(params)
    end

end
</code></pre>

I would be allowing the end-user to be able submit anything into the database as long as it lives in the nested hash of :food. This is what it would look like if the user did a simple inspect of the form on their brower:

<pre><code>
input type="text" name="title" id="title"
</code></pre>

The user could simply edit the name assignment to 'items':

<pre><code>
input type="text" name="items" id="title"
</code></pre>

And then submit to the database for 'patching'. [This was the story of How Egor Homakov hacked GitHub](http://www.zdnet.com/article/how-github-handled-getting-hacked/). He exposed the Rails mass assignment vulnerability.

<img align="center" src="http://i.memecaptain.com/gend_images/tkrRWQ.gif" alt="...">

Fortunately, Rails has patched (no pun intended) this up.

Rails disallows the application from allowing controllers to use 'weak' parameters. Strong Params are private methods set in the controller to define what will be pass through the forms and ultimately submitted into the database.

<pre><code class="ruby">

    private

    def food_params(*args)
        params.require(:food).permit(*args)
        # args can be any key in the :food hash
        # for instance, :title and/or :items
    end

</code></pre>

Set up as a private method on the bottom of the FoodsController, 'food_params' will now 'require' that the input values relate only to the :food key, and only the arguments (*args) set by the application owner can pass through to the database. Take a look at the old and new update controller actions:

<pre><code class="ruby">
OLD
class FoodsController < ApplicationController
    def update
        food = Food.find(params[:id])
        food.update(params)
    end
end

NEW
class FoodsController < ApplicationController
    def update
        food = Food.find(params[:id])
        food.update(food_params(:title))
    end

    private

    def food_params(*args)
        params.require(:food).permit(*args)
        # args can be any key in the :food hash
        # for instance, :title and/or :items
    end
end
</code></pre>

I'm only allowing for the food title to be updated in the database. If the end-user change the html code to input for food ':items' or any other key-values, my strong parameterss 'food_params' won't allow it.

And there you have it. Params aren't so hidden, and they can be quite powerful.

###### Sources
  * [Docs - Ruby on Rails - Parameters](http://api.rubyonrails.org/classes/ActionController/Parameters.html#method-c-new)
  * [Code - Rails - Strong Parameters](https://github.com/rails/strong_parameters/blob/master/lib/action_controller/parameters.rb)
  * [Youtube - How Params Work in Ruby](https://www.youtube.com/watch?v=ZV6t63JaK7k)
  * [News - Egor Homakov hacks GithHub](http://www.zdnet.com/article/how-github-handled-getting-hacked/)