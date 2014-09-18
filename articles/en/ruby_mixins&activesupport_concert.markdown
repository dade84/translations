# Ruby Mixins & ActiveSupport::Concern #

A few people have asked: what is the dealio with ActiveSupport::Concern? My answer: it encapsulates a few common patterns for building modules intended for mixins. Before understanding why ActiveSupport::Concern is useful, we first need to understand Ruby mixins.

Here we go�!

	<!--
	  a few (���������;)
	  deal with (����� ���� �;)
	  encapsulate (���������������;���������; ������ ��������; ���������;)
	  intend (�������������; ������������;)
	  here we go (�������)
	  ancestor (������;)
	  that is (�� ����;)
	-->

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

When MyModA is included in MyModB, the code in the included() hook will run, and if has_many() is not defined on MyModB things will break:

	irb :050 > module MyModB
	irb :051?>   include MyModA
	irb :052?> end
	NoMethodError: undefined method 'has_many' for MyModB:Module
	  from (irb):46:in 'included'
	  from (irb):45:in 'class_eval'
	  from (irb):45:in 'included'
	  from (irb):51:in 'include'

ActiveSupport::Concern skirts around this issue by delaying all the included hooks from running until a module is included into a non-ActiveSupport::Concern. Redefining the above using ActiveSupport::Concern:

	module MyModA
	  extend ActiveSupport::Concern

	  included do
	    has_many :squirrels
	  end
	end

	module MyModB
	  extend ActiveSupport::Concern
	  include MyModA
	end

	class MyClass
	  def self.has_many(*args)
	    puts "has_many(#{args.inspect}) called"
	  end

	  include MyModB
	# irb>
	# has_many([:squirrels]) called
	end

	# irb> MyClass.ancestors
	# => [MyClass, MyModB, MyModA, Object, Kernel, BasicObject]
	# irb> MyClass.singleton_class.ancestors
	# => [Class, Module, Object, Kernel, BasicObject]

	Great? Great.

	But why is ActiveSupport::Concern called "Concern"? The name Concern comes from AOP (http://en.wikipedia.org/wiki/Aspect-oriented_programming). Concerns in AOP encapsulate a "cohesive area of functionality". Mixins act as Concerns when they provide cohesive chunks of functionality to the targe class. Turns out using mixins in this fashion is a very common practice.

	ActiveSupport::Concern provides the mechanics to encapsulate a cohesive chunk of functionality into a mixin that can extend the behavior of the target class by annotating the class' ancestor chain, annotating the class' singleton class' ancestor chain, and directly manipulating the target class through the included() hook.

	So...

	Is every mixin a Concern? No. Is every ActiveSupport::Concern a Concern? No.

	While I've used ActiveSupport::Concern to build actual Concerns, I've used it to avoid writing out the boilerplate code mentioned above. If I just need to share some instance methods and nothing else, then I'll use a bare module.

	Modules, mixins and ActiveSupport::Concern are just tools in your toolbox to accomplish the task at hand. It's up to you to know how the tools work and when to use them.

	I hope that helps somebody.
	

