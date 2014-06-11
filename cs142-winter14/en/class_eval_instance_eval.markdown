# Understanding class_eval and instance_eval #

Two Ruby methods seem to cause more confusion than any others, particularly due to the way they are explained. These two are class_eval and instance_eval. The names are very similar, and their behavior is counterintuitive. The bottom line is this:

*	Use ClassName.instance_eval to define a class method (one associated with the class object but not visible to instances).
*	Use ClassName.class_eval to define an instance method (one that applies to all of the instances of ClassName).

To understand why this is true, let's go through some examples, starting with the following code:

	class MyClass
	  def initialize(num)
	    @num = num
	  end
	end

	a = MyClass.new(1)
	b = MyClass.new(2)

Before we get going, remember that in Ruby everything is an object. That means classes are objects too. When you define MyClass, Ruby will create a global variable with the name MyClass, which is the class object for MyClass. When you write MyClass.new you are getting the class object MyClass and then calling the new method on that object, which gives you a new instance of MyClass. Instances are the actual objects of the class that you would normally use. There is one class with many instances.

So we have two objects that are both of the same class. Of course, this class isn't very useful because it doesn't do anything. There is no way to access @num becasuse we did not define any getter or setter methods.

	irb> a.num
	NoMethodError: undefined method 'num' for #<MyClass:0x007fba5c02c858 @num="1">

Let's look briefly at instance_eval. What can we do with it? We can run code as if we were inside a method of the specific object we call it on. That means we can access instance variables and private methods. Let's evaluate an expression in the instances of MyClass so that we get the values out.

	irb> a.instance_eval { @num }
	=> 1
	irb> b.instance_eval { @num }
	=> 2
That's great, but it would be a real pain to do that a lot. Let's define a method to do it for us.

	irb> a.instance_eval do
	irb>   def num
	irb>     @num
	irb>   end
	irb> end
	=> nil
	irb> a.num
	=> 1
	irb> b.num
	NoMethodError: undefined method 'num' for #<MyClass:0x007fba5c08e5f8 @num="2">

Whoops! We used instance_eval, which only evaluates in the context of one object. We defined a method, but only on the particular object a. How do we make a method that is shared by all objects of that class? Perhaps we should define a method on the class object?
	
	irb> MyClass.instance_eval do
	irb>   def num
	irb>     @num
	irb>   end
	irb> end
	=> nil
	irb> b.num
	NoMethodError: undefined method 'num' for #<MyClass:0x007fba5c08e5f8 @num="2">

Oops, that didn't work either. What happened? Well, we did the same thing as above, but on a class object! That means we defined a method on the class object; this is not the same thing as a method that gets inherited by the objects of that class. It's just the same as if we defined a method in the class with def self.num, which is similar to a class static method in Java. This method would have to be invoked as MyClass.num, not a.num, and it won't work anyway:

	irb> MyClass.num
	=> nil

We get nil here because there is no variable @num in the MyClass object. Undefined variables have a default value of nil.

Alright, so what's the right way to do it? The answer is class_eval:
	
	irb> MyClass.class_eval do
	irb>   def num
	irb>     @num
	irb>   end
	irb> end
	=> nil
	irb> b.num
	=> 2

Horray! That worked. We defined a method for the class, not on the class object, and that method is then available for all objects of that class.

Note that we called class_eval on MyClass, not on one of the instances. Invoking class_eval on an instance wouldn't work because class_eval isn't a method of arbitrary objects, only of class objects like MyClass. But you can get an object's class dynamically with the class method:

	irb> a.class_eval
	NoMethodError: undefined method 'class_eval' for #<MyClass:0x007fba5c02c858 @num="1">
	irb> a.class.class {}
	=> nil

Another way to think about this is that class_eval is equivalent to typing the code inside a class statement:

	MyClass.class_eval do
	  def num
	    @num
	  end
	end

behaves exactly the same as the following code:

	class MyClass
	  def num
	    @num
	  end
	end


	



