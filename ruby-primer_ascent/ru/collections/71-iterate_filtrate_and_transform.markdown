# 7.1 Iterate, Filtrate and Transform #

## Move and change ##

We've already used <b>each</b> in a bunch of places, so we'll start off with an each variant called each_with_index. With this you can define blocks with two arguments, the element (just like each) and its index.

	<!--
	  start off (начинаться;)
	  just like (почти такой же как; совсем как; прямо как;)
	-->

	[4, 8, 15, 16, 23, 42].each_with_index { |e, i| puts "#{e} -- #{i}" }
 
We know about the Enumerable module and its purpose. Any class that mixes in Enumerable has access to all its methods. Hash in Ruby, just like Array, also includes Enumerable.

The first argument in the each_with_index block for hashes would contain the key-value pair as an array, the second being the pair's position in the hash. Hashes in Ruby are funny that way - iterating over a Hash almost always gives you the key and value in an Array.
	
	<!--
	  funny (странный; непонятный;)
	  iterate over (перебирать;)
	-->

	{:locke => "4", :hugo => "8"}.each_with_index do |kv, i| 
	  puts "#{kv} -- #{i}"
	end

map is another Enumerable method that is similar to each. It goes through each element in your Array or Hash and evaluates the block for that element and appends the value in an array that it returns.

In other words, <b>each</b> is used for simple iteration, <b>map</b> for transformation. In the example below, we will transform the original array, producing a new one that contains all the values of the original incremented by 1.
	
	<!--
	  similar (похожий; подобный; схожий;)
	  to evaluate (вычислять;)
	  to append (добавлять в конец;)
	  iteration (повторение;)
	  transformation (изменение; преобразование;)
	  incremented (увеличенный;)
	-->

	def map_value
	  [3, 7, 14, 15, 22, 41].map { |e| e + 1 }
	end

	p map_value

<b>map</b> returns the resultant array. <b>each</b> returns the original array.

The purpose of <b>map</b> is to return this resultant array created when the operations are applied to the original data structure -- which obviously makes its return value useful. <b>each</b> is more of a generic iterator that is often used to puts things out. Because of the simple nature of <b>each</b>, it is also used as a custom iterator builder.

Exploit the fact that <b>map</b> always returns an array: write a method hash_keys that accepts a hash and maps over it to return all the keys in a linear Array.
	
	<!--
	  resultant (результирующий; итоговый; вытекающий;)
	  obviously (явно; ясно; очевидно;)
	  generic (универсальный;общий; типичный;)
	  put out (вывести;)
	  iterator (итератор; периодизатор; программа организации циклов;)
	  because (для; из-за; )
	  nature (сущность;)
	  custom (пользовательский;)
	  builder (средство построения;)
	  exploit the fact (исользуя тот факт;)
	  linear (линейный;)
	  map (преобразовывать;)
	  map over (установить соответствие;)
	-->
	
	def hash_keys(hash)
	  hash.map {|key,value| key}
	end

<b>inject</b> is a powerful enumerator that can do iteration (walking over all items in a collection), accumulation and transformation all at once. Here's an example using an Array.

	<!--
	  accumulation (суммирование;)
	  all at once (все сразу;)
	  iterated (повторный;)
	-->	

	[4, 8, 15, 16, 23, 42].inject(0) do |accumulator, iterated|
	  accumulator += iterated
	  accumulator
	end


There are bunch of things going on here.
* The 0 argument to the inject(0) method is an optional default value.
* This optional default value is assigned to the first argument accumulator in the block. If there is no value, then accumulator starts with the first value of the array. This is done only once on the first iteration.
* inject is also an iterator, the second argument iterated being the element it's currently on.
* accumulator here, is basically incrementing itself by adding the value of iterate to itself.

If you find this hard to wrap your head around, then here's a more broken down version of what <b>inject</b> essentially does, using <b>each</b>. It takes the Array and an optional default value, and assigns that default value to a local variable accumulator. It assigns the first element of the array if there is no default value, i.e. it's nil. It then loops through the array and increments the local variable accumulator and finally returns the new value of accumulator.
	
	<!--
	  optional (дополнительный; необязательный;)
	  wrap head around (разобраться; сообразить; понять;)
	  break down (анализировать;)
	  essentially (по существу; существенно; в основном;)
	-->

	def custom_inject(array, default = nil)
	  accumulator = default || array[0]
  
	  array.each do |element|
	    accumulator = accumulator + element
	  end
  
	  accumulator
	end

	p custom_inject([4, 8, 15, 16, 23, 42], 0)

<b>inject</b> greatly simplifies the amount of code you write when you're building values and iterating at the same time. Building a Hash or an Array is a pretty common use case.

	<!--
	  simplify (упрощать; сокращать;)
	-->

	[4, 8, 15, 16, 23, 42].inject({}) { |a, i| a.update(i => i) }

Try implementing a method called occurrences that accepts a string argument and uses inject to build a Hash. The keys of this hash should be unique words from that string. The value of those keys should be the number of times this word appears in that string.

	def occurrences(str)
  	  str.scan(/\w+/).inject(Hash.new(0)) do |hash,item|
            hash[item.downcase] += 1
            hash
  	  end
        end

	The inject method is also aliased to reduce.

	<!--
          respective (соответственный; относительный; соответствующий)
        -->

## Don't ask  to ask ##

all?, any? and none? are some useful querying enumerators that always return true or false.

	<!--
	  querying запрос
	-->  

These enumerators are principally similar. They return true if any, all or none of the items match the condition in the block. false, otherwise.

	[4, 8, 15, 16, 23, "42"].any? { |e| e.class == String }

At least one of the elements in that list is a String, so any? returns true.

With a Hash, you can use these in two ways. Either with one argument that is a 2 element array of the key-value pair. candidate[0] is the key and candidate[1] is its value.

	{:locke => 4, :hugo => 8}.any? { |candidate| candidate[1] > 4 } 

This returns true because the value of the second candidate :hugo is greater than 4. You can also use these with two arguments.

	{:locke => 4, :hugo => 8}.any? { |candidate, number| number < 4 } 

You get the general idea on how to use these enumerators. Now try to create an Island and seed it with an Array of a few candidates that will protect it. The island will survive only if none of the candidates is "Esau" and it'll remain safe only if all of the candidates are "Jack".

	<!--
	  seed (распределять; отбирать;)
	  survive (остаться в живых; выжить; сохраниться;)
	  remain (сохранить; сохраниться; оставаться;)
	  safe (безопасный;)
        -->

	class Island
	  def initialize(candidates)
	    @candidates = candidates
	  end
  
	  def survive?
	    @candidates.none?{candidate == "Esau"}
	  end
  
	  def safe?
	    @candidates.all?{candidate == "Jack"}
	  end
	end

## Growth, decay and transformation ##

Ruby lets you treat Arrays in a manner similar to Sets by providing the Union, Intersection and Difference operations between two arrays.

The | (pipe character) is the Union operator. It joins two arrays and returns the result with duplicates removed.

	<!--
	  growth (возрастание;)
	  decay (убывание;)
	  treat (трактовать; интерпретировать;)
	  intersection (пересечение; логическое умножение; конъюнкция &)
	  difference (дополнение; дизъюнкция)
	-->

	union_example = ["a", "b", "a"] | ["c", "c"]
	p union_example

The & operator is the Intersection operator. It is an easy way to find elements that are common to two (or more) arrays. Similar to Array union, all the elements in the result of an intersection will be unique.

Finding the difference between two arrays is yet another useful operation. The Official Ruby Documentation defines it thus: "Returns a new array that is a copy of the original array, removing any items that also appear in other_ary. (If you need set-like behavior, see the library class Set.)"

There is some nuance to that definition. Consider what happens when you do this operation:

	<!--
	  nuance (тонкость; нюанс;)
	  consider (рассматривать;)
	-->

	array_difference = [1,2,3, 1,2,3] - [1]
	p array_difference

Where did all the 1s go?

The - method preserves even the duplicate elements in the original array, however all occurence of each element in second array are removed from the result - including the duplicates.

You might have noticed that the definition of the Array difference operator - mentions the class Set. Set is a standard Ruby class that provides the semantics of the mathematical notion of Set. Apart from the standard union, intersection and difference operations, it lets you group elements of a set according to arbitrary conditions through the <b>classify</b> method. You can also <b>divide</b> a set into subsets using the divide method. Refer to The Official Ruby Documentation on the Set class for more information on Sets.

Let me conclude this section by a quick exercise. Solve it using one or more of the Array operations described above.

You are running a promotion for your e-commerce operation. The promotion became wildly successful and you are running out of inventory for many items. To process the orders, you have to remove items that are out of stock from the customer's order. Also, you promised your customers to ship some free gifts with every order, don't forget that!
	
	<!--
	  preserve (сохранять; оберегать;)
	  occurence (вхождение;)
	  notion (обозначение;)
	  apart from (наряду с;)
	  arbitrary (произвольный; самовольный;)
	  classify (группировать; классифицировать;)
	  set into (вставлять;)
	  promotion (рекламная кампания;)
	  wildly (дико; сильно;)
	  inventory (инвентаризация;)
	  run out of inventory (закончиться на складах;)
	-->
	
	class Order
	  GIFT_ITEMS = [Item.new(:big_white_tshirt), Item.new(:awesome_stickers)]
	  OUT_OF_STOCK_ITEMS = [Item.new(:ssd_harddisk)]

	  def initialize(order)
	    @order = order || []        
	  end
  
	  def final_order
	    @order - OUT_OF_STOCK_ITEMS | GIFT_ITEMS
	  end
	end

	customer_order = Order.new([Item.new(:fancy_bag),Item.new(:ssd_harddisk)])

	p customer_order.final_order


