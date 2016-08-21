---
layout: post
title: thought1 = Thought.new
---

Bear with me as I create or code a thought.

<pre><code data-trim="" class="ruby">
class Thought

@@owner = "Tony"

attr_reader :topic, :notion, :date

	def initialize(topic, notion, date)
		@topic = topic
		@notion = notion
		@date = date
	end

	def self.owner
		@@owner
	end

end

thought1 = Thought.new("hello world", "I guess this is my first post.", 2016-07-30)

</code></pre>

That is a thought. Well, according to me and my newfound knowledge of Ruby Classes, the above snippet is the Thought Class, of which I can use to create a thought, an instance, an object.

Let's break it down, and just maybe I'll have created a very narrow and limited piece of Artificial Intelligence code (one can hope).

```ruby
class Thought
#=> This is the name and creation of my class

@@owner = "Tony"
#=> This class constant can't be changed, so it's apt that I use it to declare it's "owner", me. These are my thoughts.

attr_reader :topic, :notion, :date
#=> These class attributes are now accessible as instance methods (i.e. thought.topic => the topic). These are setter methods and thus can't be changed like a getter method.

	def initialize(topic, notion, date)
	#=> Here I'm declaring new "thoughts" to be instanstiated with topic, notion (actual thought), and date. When a new thought (Thought.new) is created, these instance variables are created as well. These allow for my getter methods to work.
		@topic = topic
		@notion = notion
		@date = date
	end

	def self.owner
	#=> This allows me to call Thought.owner to return the value or string I've set @@owner to be.
		@@owner
	end

end

thought1 = Thought.new("hello world", "I guess this is my first post.", 2016-07-30)
#=> This should instantiate my first thought as thought1 taken in the topic, actual thought, and date.


```

Over the course of my coding journeys, I hope I can return to this class and re-define, re-factor, and re-vise it. Maybe, this will also serve as a re-minder of who and what I am: forever a learner, a student.

Please comment below or anywhere and help me grow as a learner. Thanks!




