# 4.1 The 'inlcuded' Callback and the 'extend' Method #

## The 'included' callback ##

We covered Modules in The RubyMonk Primer: Modules chapter. You should finish that lesson if you haven't done so already.

In this section we'll discuss the included method. It is a callback that Ruby invokes whenever the module is included into another module/class. An example should make this clear:

	module Foo
	  def self.included(klass)
	    puts "Foo has been included in class #{klass}"
	  end
	end

	class Bar
	  include Foo
	end

Note that included is defined in the module level using the self keyword.

included is going to become useful in an exercise down the line, but for now, that is pretty much it!

	<!--
	  to cover (расссматривать;)
	  down the line (в будущем; в дальнейшем;)
	  pretty much (в достаточной степени;)
	-->

## Welcome to Module#extend ##

We'd discussed about include method in the The RubyMonk Primer: Modules chapter. include makes it possible to share behaviour through modules by mixing in the methods from a module into a class.

	module Foo
	  def self.module_level_method
	    puts "Module level method"
	  end
	end

	class Bar
	  include Foo
	end

	Bar.module_level_method

As you can see, we receive a NoMethodError when trying to invoke the module level method from the class.

But there are occasions when you would want to include some methods from a module as class-level methods in a class. This is where the method extend becomes useful.

The extend method works similar to include, but unlike include, you can use it to extend any object by including methods and constants from a module.

Here is an example:

	module Foo
	  def module_method
	    puts "Module Method invoked"
	  end
	end

	class Bar
	end

	bar=Bar.new
	bar.extend Foo
	bar.module_method

The important thing to note here is that extend works everywhere: inside a class/module definition and on specific instances. include however cannot be used on specific objects.

In a sense, extend can be used to mimic the functionality of the include method. Let us discuss this aspect a bit: when you add and include statement inside a class definition, Ruby ensures that all new instances of the class will have the methods defined in the included module.

So, to mimic include, you'll have to reproduce what Ruby does for you when using include. That means you'll have to add all the methods present in the mixin module to every new instance of the class. The next exercise is to do just that!

These bits of information will help you with the exercise:

* Ruby always calls the instance method initialize whenever it builds a new instance of a class
* The self keyword can be used to get a handle on the current instance - even inside the initialize method
* Once we get a handle on the instance when it is being created, we can just extend the module and achieve the desired result

	<!--
	  In a sense (В некотором смысле; в известном смысле;)
	  to mimic (имитировать; подражать;)
	  to ensure (гарантировать; заверять; )
	  bits of information (факты;)
	  handle (ссылка; идентификатор;)
	  Once (как только;)
	  instance (момент;)
	-->

Well, that is all you need to know! Now get going and make those tests green!

	module Foo
	  def method_in_module
	    "The method defined in the module invoked"
	  end
	end

	class Bar
	  def initialize
	  end
	end

That should have been an interesting exercise!

However, it was meant to only demonstrate how you can use extend to mimic include. You should not use that approach where the include method would work. include is the standard Ruby idiom for mixing in methods from a module to a class and you should stick to it.

	<!--
	  meant to (нацелены на; предназначены для;)
	  to stick (придерживаться;)
	-->

## Using 'extend' for adding class level methods ##

In the previous section, you saw how extend can mimic the functionality that include provides. But that is not all! extend is a badass; it can add class level methods - something that include can't do.

By this point you should be able to imagine how you can use extend to do that. The magic is in treating everything as an object - as you may extend and instance of a class, you may extend the class itself! Take a look:

	module Foo
	  def say_hi
	    puts "Hi!"
	  end
	end

	class Bar
	end
	
	Bar.extend Foo
	Bar.say_hi

Great. Lets do a quick recap now. So far in this lesson, we've learned:

* The include method has a callback which is invoked whenever a module is included into another module/class - the included method. It is executed in the context of the mixin module and should be defined there. Its signature is self.included(base) where base is the target class.
* While extend can be used to add instance methods to instances, we don't use that much. However, unlike include, extend can be used to add methods to the classes themselves.

	<!--
	  badass ("вещь")
	  to treat (трактовать;)
	  quick recap (краткий повтор;)
	-->


I want you to use the above two points to solve the following challenge. You have to modify the module Foo in the following exercise so that when you include it the class Bar, it also adds all the methods from ClassMethods into Bar as class methods.

	module Foo
	  def self.included(base)
	    base.extend ClassMethods
	  end
	  module ClassMethods
	    def guitar
	      "gently weeps"
	    end
	  end
	end

	class Bar
	  include Foo
	end

	puts Bar.guitar

The exercise you just solved is a commonly used Ruby programming idiom. It would come in handy when you need to write modules that also can add class level methods to the target class.

	<!--
	  I want you to do smth (complex object) (я хочу, чтобы вы сделали что-то;)
	  challenge (сложная задача; проблема;)
	  idiom (характерный для данного языка оборот; особый стиль;)
	  come in handy (пригодиться; быть кстати;)
	-->
	  
