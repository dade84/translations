# 4.2 Wrapping Up Modules #

## The 'extended' callback ##

Just like included which is a callback for include, there is a callback for extend and it is appropriately called extended.

We'll breeze through it using a simple example:

	module Foo
	  def self.extended(base)
	    puts "Class #{base} has been extended with module #{self} !"
	  end
	end

	class Bar
	  extend Foo
	end

	<!--
	  appropriately (соответственно;)
	  to breeze (промчаться;)
	-->

## Namespacing non-OO code using module level methods ##

Object Oriented programming is fundamentally about grouping data and its related behaviour together.
We call the data the 'state' of the object and any logic related to the state as the 'behaviour' of the object.

It is also possible (but not advisable) to write non-object oriented code in Ruby by avoiding creation of objects and instead relying on independent methods. Ruby's Math module is an example of a collection of such methods. This is how you can define and use a similar module:

	module Weather
	  def self.will_it_rain_on(date)
	    "it depends"
	  end
	end

	puts Weather.will_it_rain_on(Date.today)


	<!--
	  be about (осуществлять; предполагать;)
	  to group (объединять; объединяться;)
	  group together (объединяться; группировать; связывать;)
          advisable (рекомендуемый; желательный; целесообразный;разумный;)
	  to avoid (избегать; сторониться; уклоняться;)
	  rely on (ссылаться на;)
	-->

A quick dirll before we move on. In the following exercise you have to define a static method square in the module Math. It should obviously return the square of the number passed to it. Alright, jump right in:

	<!-- 
	  obviously (очевидно; ясно;)
	  jump in (впрыгивать;)
	-->

	module Math
	  def self.square(number)
	    number*number
	  end
	end

	puts Math.square(6) 

