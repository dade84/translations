# Ruby Mixins & ActiveSupport::Concern #

A few people have asked: what is the dealio with ActiveSupport::Concern? My answer: it encapsulates a few common patterns for building modules intended for mixins. Before understanding why ActiveSupport::Concern is useful, we first need to understand Ruby mixins.

Here we go…!

## First, the Ruby object model: ##
* http://blog.jcoglan.com/2013/05/08/how-ruby-method-dispatch-works/
* http://www.ruby-doc.org/core-1.9.3/Class.html

As you can see mixins are "virtual classes" that have been injected in a class's or module's ancestor chain. That is, this:

	module MyMod
	end

	class Base
	end

	class Child < Base
	  include MyMod
	end

	# irb> Child.ancestors
	# irb> => [Child, MyMod, Base, Kernel, BasicObject]

results in the same ancestor chain as this:

	class Base
	end

	class MyMod < Base
	end

	class Child < MyMod
	end

	# irb> Child.ancestors
	# => [Child, MyMod, Base, Object, Kernel, BasicObject]

Great? Great.

Modules can also be used to extend objects, for example:

	my_obj = Object.new
	my_obj.extend MyMod

	# irb> my_obj.singleton_class.ancestors
	# => [MyMod, Object, Kernel, BasicObject]

In Ruby, every object has a singleton class. Object#extend does what Module#include does but on an object's singleton class. That is, the following is equivalent to the above:
	
	my_obj = Object.new
	my_obj.singleton_class.class_eval do
	  include MyMod
	end

	# irb> my_obj.singleton_class.ancestors
	# => [MyMod, Object, Kernel, BasicObject]

This is how "static" or "class" methods work in Ruby. Actually, there's no such thing as static/class methods in Ruby. Rather, there are methods on a class's singleton class. For example, w.r.t. the ancestors chain, the following are equivalent:

	class MyClass
	  extend MyMod
	end

	# irb> MyClass.singleton_class.ancestors
	# => [MyMod, Class, Module, Object, Kernel, BasicObject]

	class MyClass
	  class << self
	    include MyMod
	  end
	end

	# irb> MyClass.singleton_class.ancestors
	# => [MyMod, Class, Module, Object, Kernel, BasicObject]

Classes are just objects "acting" as "classes".

## Back to mixins... ##

Ruby provides some hooks for modules when they are being mixed into classes/modules:
* http://www.ruby-doc.org/core-1.9.3/Module.html#method-i-included
* http://www.ruby-doc.org/core-1.9.3/Module.html#method-i-extended

For example:

	module MyMod
	  def self.included(target)
	    puts "included into #{target}"
	  end

	  def self.extended(target)
	    puts "extended into #{target}"
	  end
	end

	class MyClass
	  include MyMod
	end

	# irb>
	# included into MyClass

	class MyClass2
	  extend MyMod
	end
	
	# irb>
	# extended into MyClass

	# irb> MyClass.ancestors
	# => [MyClass, MyMod, Object, Kernel, BasicObject]
	# irb> MyClass.singleton_class.ancestors
	# => [Class, Module, Object, Kernel, BasicObject]

	# irb> MyClass2.ancestors
	# => [MyClass2, Object, Kernel, BasicObject]
	# irb> MyClass2.singleton_class.ancestors
	# => [MyMod, Class, Module, Object, Kernel, BasicObject]

Great? Great.

## Back to ActiveSupport::Concern... ##

Over time it became a common pattern in the Ruby worldz to create modules intended for use as mixins like this:

	module MyMod
	  def self.included(target)
	    target.send(:include, InstanceMethods)
	    target.extend ClassMethods
	    target.class_eval do
	      a_class_method
	    end
	  end

	  module InstanceMethods
	    def an_instance_method
	    end
	  end

	  module ClassMethods
	    def a_class_method
	      puts "a_class_method called"
	    end
	  end
	end

	class MyClass
	  include MyMod
	end

	# irb> MyClass.ancestors
	# => [MyClass, MyMod::InstanceMethods, MyMod, Object, Kernel, BasicObject]
	# irb> MyClass.singleton_class.ancestors
	# => [MyMod::ClassMethods, Class, Module, Object, Kernel, BasicObject]

As you can see, this single module is adding instance methods, "class" methods, and acting directly on the target class (calling a_class_method() in this case).

ActiveSupport::Concern encapsulates this pattern. Here's the same module rewritten to use ActiveSupport::Concern:

	module MyMod
	  extend ActiveSupport::Concern

	  included do
	    a_class_method
	  end

	  def an_instance_method
	  end

	  module ClassMethods
	    def a_class_method
	      puts "a_class_method called"
	    end
	  end
	end

You'll notice the nested InstanceMethods is removed and an_instance_method() is defined directly on the module. This is because this is standard Ruby; given then ancestors of MyClass ([MyClass, MyMod::InstanceMethods, MyMod, Object, Kernel]) there's no need for a MyMod::InstanceMethods since methods on MyMod are already in the chain.

So far ActiveSupport::Concern has taken away some of the boilerplate code used in the pattern: no need to define an included hook, no need to extend the target class with ClassMethods, no need to class_eval on the target class.

The last thing that ActiveSupport::Concern does is what I call lazy evaluation. What's that?

## Back to Ruby mixins... ##

Consider:

	module MyModA
	end

	module MyModB
	  include MyModA
	end

	class MyClass
	  include MyModB
	end

	# irb> MyClass.ancestors
	# => [MyClass, MyModB, MyModA, Object, Kernel, BasicObject]

Let's say MyModA wanted to do something special when included into the target class, say:

	module MyModA
	  def self.included(target)
	    target.class_eval do
	      has_many :squirrels 
	    end
	  end
	end


	

