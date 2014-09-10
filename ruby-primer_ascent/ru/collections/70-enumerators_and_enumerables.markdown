# 7.0 Enumerators and Enumerables #

## Garrison ##

"Enumerable" is Ruby's way of saying that we can get elements out of a collection, one at a time.

e-nu-mer-ate Verb:
1. Mention (a number of things) one by one.
2. Establish the number of.

An "enumerator", then, a the tool we can use to get each element out of a collection in this way. Before we go ahead, it's important to understand what Enumerable and Enumerator are in Ruby terms. Let's look at some examples.

First, calling each on the Array [4,8] in this example, returns an Enumerator object.

	[5,3].each

This is different from how we've been usually using each.

	[4,8,15,16,23,42].each { |e| puts e }


	<!--
	  enumerable (перечислимый; счетный;)
	  enumerate (перечислять;)
	  mention (упоминать;отмечать;приводить;)
	  a number of things (ряд вещей;)
	  establish the number of (установить количество;)
	  enumerator (счетчик;перечисления; перечислитель;)
	  one at a time (по одному за раз;)
	-->


Enumerable is a module used as a mixin in the Array class. It provides a number of enumerators like map, select, inject. The Enumerable module itself doesn't define the each method. It's the responsibility of the class that is including this module to do so.

This Array class, consequently, defines the each method. It returns an object of the type Enumerator when no block is given (like our first example). It yields a value of the type self when there is one (the original array in the next example).

What's the point of returning the Enumerator then?

	<!--
	  responsibility (обязанность; обязательство; ответственность;)
	  consequently (следовательно; поэтому; в результате;)
	  yield (производить; давать; возвращать; выдавать;)
	-->	

	enumerator = [3,7,14].each
	enumerator.each { |e| puts e + 1 }

Enumerator is an objectification of enumeration. The point of these methods returning these enumerators is to allow us to chain operations indefinitely and make more heavy-duty collections.

	<!--
	  objectification (воплощение;)
	  indefinitely (неопределенно; неясно; на неопределенный срок;)
	  heavy-duty (тяжелый;)
	-->

Here we chain the initial each_with_index enumerator with select to build an enumerator that returns an array of elements whose values are smaller than their index positions.

	<!--
	  initial (начальный; исходный; предварительный;)
	-->

	enum = [0, -1, 3, 2, 1, 3].each_with_index
	p enum.select { |element, index| element < index }

Try and implement a simple map_with_index on the Array class through which you can call a block with two arguments: the element and its index. It should return an Enumerator object if no block is given, an Array otherwise.

	class Array
	  def map_with_index(&block)
	    self.each_with_index.map(&block)
	  end
	end

You are good to delve deeper into the Enumerable class and learn how and when to make use of these collections.

