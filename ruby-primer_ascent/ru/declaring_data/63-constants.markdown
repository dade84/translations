# 6.3 Constants #

## A loose definition ##

Constants in Ruby are defined by convention. In the same way instance variables come into existence when you name a variable with an "@" prefix, constants come into existence when you name a *cough* variable *cough* with an upper-case character prefix. I use the term "variable" so sloppily here because Ruby is a bit strange about constants. Try changing the value of Argument::Truth in the following exercise.

	<!--
	  come into existance (возникнуть; возникать; появиться; начать существование; появитья на свет;)
	  cough (кашель; чихание;)
	  term (термин; выражение;)
	  sloppily (небрежно;)
	-->

	Argument::Truth = "No, you're not."

Strange, no? You can't see it in the STDOUT above, but Ruby will at least complain with a warning something like this: warning: already initialized constant Truth. This works because Ruby is forgiving about assigning a constant value twice statically. Because classes and modules are constants, this is helpful if one accidentally requires the same file into a program twice.

If you try to redeclare a constant dynamically, however, Ruby will get upset:

	<!--
	  loose definition (нестрогое определение; произвольное определение;)
	  at least (по крайней мере;)
	  complain (жаловаться; выражать недовольство; )
	  forgive (прощать; оставить без внимания; извинять;)
	  upset (растройство; опрокидывание; огорчение;)
	-->

	Argument::Truth = "Yes, I am."

	def rewrite_history
		Argument::Truth = "No, you're not."
	end

	rewrite_history

Note that one not need attempt to redefine a constant to get this error. Even if you delete the first line in the above example, it will fail with the same error. Ruby does not want you to assign (or re-assign) constants while your program is running -- and that's a good thing.

	<!--
	  attempt (пытаться; пробовать;)
	-->

## Classes and modules: less special than they look ##

You've probably noticed that Ruby requires you use a constant when naming classes and modules. (If not, try naming a class with a lower-case character.) There is a way around this, however, and in its use we can see that classes and modules themselves are types in the Ruby class hierarchy, just like strings and arrays.

	<!--
	  way around this (способ обойти это;)
	-->

	fence = Module.new do
	  def speak
	    "I'm trapped!"
	  end
	end

	class Sheep
	  def speak
	    "Baaaaahhhhh."
	  end
	end

	dolly = Sheep.new
	dolly.extend(fence)
	puts dolly.speak

As with many of Ruby's "whoa, what? you can do that?" features, you will find this syntax most helpful when you begin metaprogramming. For now, this syntax is useful as a tool to teach about what Ruby looks like on the inside... and that, in fact, less magic is happening than would appear at first.

Try creating classes in these exercises the same way.

	def awkward_sheep
	  sheep = Class.new do
	    def speak
    	      "Bah."
 	    end
	  end
	end

Notice that we had to wrap our sheep class in a method to get access to it from our tests. Try namespacing it as you would normally in this next exercise. You'll find you run into a new issue.

	module Fence
	  sheep = Class.new do
	    def speak
	      "Bah."
	    end 
	end

	def call_sheep
  	  Fence::sheep.new.speak()
	end

One wouldn't think there would be so much to explore in the creation of constants! But now you are armed with an understanding of how they work in your regular Ruby programs and you'll be prepared to tackle the even stranger bits once you start metaprogramming.

	<!--
	  to find (увидеть)
	  run into (столкнуться; встретиться;)
	  tackle (разбираться;)
	  bits (поведение;)
	  once (раз;)
	-->
