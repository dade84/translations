# 7.2 Building Collections  

## What is a Collection anyway?

Ruby provides two basic data structures to help you manipulate collections of objects: the Array and the Hash. These structures are the building blocks with which you can construct even advanced collections with custom behaviour. In this chapter, you will learn how to build custom Ruby collections, inspired from the simplest collection of all: the Array.

Before we delve into the matter, let us review how an Array works. Put simply, a Ruby Array is a collection of objects.

In Ruby, the standard operation all collections perform is the retrieval of elements from the collection sequentially using the <b>each</b> method. We have already seen how you can use <b>each</b> on an Array in The RubyMonk Primer on Arrays. <b>each</b> accepts a block of code as an argument. It iterates through all elements of the collection and for every element, executes the code.

    [1,2,3].each do |number|
      puts number
    end
    
Here, the each method accepts as a parameter a block of code that puts the number. Since there are three numbers in the list, each will execute the same block of code thrice, but passes successive numbers to the block for each execution.

    <!--
      inspired (вдохновленный;)
      the matter (неприятности; трудности;)
      review (обзор;)
      Put simply,.... (проще говоря;)
      retrieval (поиск и выборка;)
      successive (последовательный; последующий; успешный;)
    -->


## that magic thing: 'each'. make it your own.

Wonder how this is done? How does each implement this functionality? There is no magic to it! If you guessed <b>yield</b>, you are right. yield is a simple yet powerful Ruby construct which we've already covered in the Ruby Primer. Refer to the RubyMonk Primer: Lambdas and Blocks in Ruby lesson if you need a quick refresher.

Okay, time for a simple exercise. I have a FibonacciNumbers collection class that holds the first ten Fibonacci numbers. You need to implement the each method for this class. The method should iterate over the NUMBERS array and yield each element.

    <!--
      guess (догадываться; полагать; угадывать;)
    -->

    class FibonacciNumbers
      NUMBERS = [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]

      def each
        NUMBERS.each {|item| yield item}
      end
    end

    f=FibonacciNumbers.new
    f.each do |fibonacci_number|
      puts "A Fibonacci number multiplied by 10: #{fibonacci_number*10}"
    end


## What comes after #each ?

The each method is pretty useful in itself. But there are quite a few other methods that we usually need to work with a collection. Array#map, Array#select and Array#reject are some of those. These methods are part of Arrays as well as Hashes in Ruby. In fact, every collection is generally expected to have these methods implemented. This section is going to cover implementing one such operation: The select method.

select is a 'filtering' method. There are some sample examples in the Ruby Primer at The RubyMonk Primer: Arrays Basics lesson. As you can see, it takes as an argument a block that evaluates to true or false, and then returns the filtered collection.

Now let me show how we can implement the select method over your custom FibonacciNumbers collection.

    <!--
      in itself (сам по себе;)
      quite a few (довольно много; некоторое количество;)
      generally (как правило;)
    -->

    class FibonacciNumbers
  
      NUMBERS = [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]

      def select(&filtering_condition_block)
        filtered_result = []
        NUMBERS.each do |number|
          filtered_result << number if filtering_condition_block.call(number)
        end
        filtered_result
      end

    end

    # print only the even Fibonacci numbers
    nums = FibonacciNumbers.new
    nums.select {|num| num % 2 == 0}.each {|num| puts num}
    
The above example uses an explicit block and calls it instead of yielding an implicit block. That was done to make the variable names more explict in the code and convey the intent better. If you don't know it already, work through the Ruby Primer: Ascent - Blocks chapter to understand the difference between implict and explicit blocks.

    <!--
      convey (перевозить; передавать;)
      intent (намерение; цель; умысел; желание; смысл; суть;)
    -->
    
    
## Coming full circle

Now that you understand how select works, look at some other methods that are commonly used to work with collections: reject, map, inject, find and so on. In fact, select was only the beginning. There are numerous methods that are needed to work effectively with collection objects. Reimplementing all of them every time you need to build a collection is tedious and is not the best way to spend your time.

However, The Ruby language provides an easy and effective way out. If your collection object implements the each method, Enumerable - a standard Ruby module can implement the rest of the collection methods for you. Just add the line include Enumerable to your class and that will provide your class with all the standard collection methods.

Do you remember what the include statement does? It makes all the instance methods of the module available as instance methods for your class as well. If you haven't gone through it already, try The RubyMonk Primer on Modules.

The Enumerable module comes with numerous methods that greatly helps when working with collections. A full list of all the methods including select, reject, map, inject and more are in The Official Ruby Documentation on Enumerables.

To recap: You can make an object a collection by defining the each method and mixing-in the Enumerable module into the class. All of the Enumerable methods rely on each which should be implemented by the host class.

Okay, here is possibly one of the simplest exercise in RubyMonk yet! Run the code snippet and make the tests pass!

    <!--
      tedious (скучный; утомительный; громоздкий;)
      way out (выход; исход;) 
      numerous (многочисленный;)
      rely (полагаться; надеяться; зависеть; опираться;)
      code snippet (фрагмент кода;)
    -->

    class FibonacciNumbers
      include Enumerable 
  
      NUMBERS = [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
  
      def each(&block)
        NUMBERS.each{|number| block.call(number)}
      end
    end

    f = FibonacciNumbers.new
    if f.respond_to?(:map)
      squares = f.map {|number| number * number }
	    puts "The squares of the fibonacci numbers are #{squares}"
    else
      puts "I'll reveal the squares to you once you pass the tests."
    end
    
That is all! Next time you've a custom object you want to convert to a standard Ruby collection, just implement your each method and include the Enumerable mixin, and you have a full-blown Ruby Collection raring to go!

    <!--
      full-blown (развитый;)
      raring (жаждущий; полный нетерпения;)
    -->
