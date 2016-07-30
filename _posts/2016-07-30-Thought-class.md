---
layout: post
title: thought1 = Thought.new
---

Bear with me as I create or code a thought.

```ruby
class Thought

@@owner = "Tony"

attr_reader :topic, :notion, :date

	def initialize(topic, notion, date)
		@topic = topic
		@notion = notion
		@date = date
	end

end

thought1 = Thought.new("hello world", "I guess this is my first post.", 2016-07-30)

```

