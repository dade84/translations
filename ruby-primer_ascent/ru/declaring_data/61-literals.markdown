# 6.1 Literals #

## Review ##

You've already seen literals: every time you type an object directly into Ruby code, you're using a literal. A literal, in terms of data declaration, is simply when the input value (the code you type) is exactly equal to the output value (how that value is printed). Take a look at the following example:

	puts [:i, :am, :a, :literal, :array].inspect

	<!--
	  literals (литералы;)
	  review (обзор;)
	  in terms of (относительно; касательно; в терминах; как;)
	-->

Rubymonk's STDOUT is slightly different from that of irb (if you're following along at home). If you have your own Ruby process running, you'll notice that calling .inspect on the array prints the array literal within a string literal. In rubymonk, the string is stripped but the effect is the same -- you can see that when Ruby outputs an array, it looks like like the array we typed ourselves.

	<!--
	  slightly (слегка; немного; )
	  within (в;)
	  to strip (лишать;)
	-->

Try it a few times yourself. Each of the methods below should return the literal it describes:

	def an_array_with_5_elements
	  [1,2,3,4,5]
	end

	def a_string_longer_than_10_characters
	  "a_string_longer_than_10_characters"
	end

	def a_number_with_a_decimal_place
	  1.0
	end

	def an_array_of_hashes
	  [{:a => 1},{:b => 2}]
	end

	def an_array_of_arrays
	  [["blank","blank","blank"],["gummy","blank","blank"],["blank","blank","blank"]]
	end

Click on "See the Solution" above -- notice how array and hash literals don't care about whitespace. This can come in handy when we want to format our Ruby data like a document -- notice that the an_array_of_hashes and an_array_of_arrays method bodies already look somewhat tabular.

	<!--
	  come in handy (оказаться кстати; удачно подвернуться; быть полезным;)
	  somewhat (немного; слегка; отчасти;)
	  tabular (в виде таблиц; в табличной форме;)
	  whitespace (пробел; разделитель; неотображаемый символ;)
	-->

## A few new tricks ##

Thankfully, the Ruby's literal syntax is usually exactly what we expect: Type a number, get a number. Type a string, get a string. Literals are not without nuance, however. Here are some forms you might not guess. Let's start with numbers. Everything starts with numbers.

	<!--
	  trick (прием;)
	  thankfully (слава богу; к счастью;)
	  nuance (тонкость; нюанс;)
	  however (несмотря на это;)
	-->

	def describe(something)
	  puts "I am a: #{something.class} and I look like: #{something}"
	end

	describe(1_024)
	describe(1.2e-30)

The e-30 in the above example is scientific notation for "less three decimal places". The underscore, however, means absolutely nothing; you'll notice it wasn't printed when that code was executed. It's simply a convenience for writing out large numbers which can become difficult to read without demarcation. Try using underscores to make a huge, readable number. Assume you have access to the describe method from here on out. We've hidden it to avoid cluttering up the next few examples and exercises.

	<!--
	  scientific notation (экспоненциальная запись;)
	  decimal places (количество знаков после запятой;)
	  underscore (нижнее подчеркивание; символ "_";)
	  demarcation (разграничение; межевание; установление границ;)
	  assume (предполагать; допускать;)
	  clutter up (захламлять; наваливать; перегружать; приводить в беспорядок;)
	-->


	def big_num
	  9_9_99999999999999999_999_99999999999999999_999999999
	end

	describe(big_num)

Pretty easy! And that's really the whole point... it should be easy to create a new object of the types we use everywhere, such as numbers, arrays, and strings. Speaking of strings! Let's take a look at a couple of forms you may have not encountered in your Ruby programming yet.

	<!--
	  whole point (весь смысл;)
	  to encounter (встретить; встретиться; наталкиваться; сталкиваться;)
	-->

	describe("a string with a sci-notation float! #{1.2e-3}")
	describe('a boring, non-interpolated string. #{1.2e-3}')
	describe("a backslash-escaped string: \#{1.2e-3}")


Try to guess the string literal forms required for the following three exercises. The first two will use a technique you just saw (escaping). The third exercise might require a hint because it's a little weird. See if you can get it without the hint, though! If you've been reading other Ruby code, you may have seen it elsewhere.
	
	<!--
	  to guess (предполагать; догадываться; угадывать; угадать;)
          technique (техника; метод;методика;)
	  escaping technique (техника экранирования;комбинирования; изолирования;)
	  hint (намек; совет; тень; подсказка;)
	  weird (странный;)
	  though (несмотря на это;)
	-->

	def quoted_string(to_be_quoted)
	  "Suuuure. You were just \"#{to_be_quoted}\"."
	end


	def multi_line_string(*lines)
	  "Here are your lines!\n\n#{lines.join("\n")}"
	end

	def big_q_string(numerator, denominator)
	  %Q[This %Q syntax is the ugliest one.
	#{numerator} out of #{denominator} "dentists" agree.]
	end

## Collections ##

The Array literal makes a lot of sense once you've seen it: [1, 2, 3]. It's quite common to create arrays of strings, however, and typing all those quotes can be a nuisance. They also make things harder to read if they just appear repetitive. The array-of-strings literal is similar to the big-q literal you just saw. In fact, it's so similar... heck, why don't you try to guess it?

	<!--
	  a lot of sense (много смысла)
	  once (стоит только) 
	  common (распространенный;)
 	  nuisance (неприятность; помеха; неудобство;)
          however (при этом;)
	  appear (оказываться;)
	  repetitive (повторяемый;)
	  heck (черт возьми;)
	-->

	def repetitive_array_of_strings
	  ["Wow,", "this", "is", "a", "pretty", "long", "list", "of", "words", "and", "it", "took", "me", "a", "long", "time", "to", "type", "because", "of", "all", "those", "darn", "quote", "characters.", "Geez."]
	end

	def array_of_words_literal
	  %w[With this double-u shorthand it wasn't very hard at all to type out this list of words. Heck, I was even able to use double-quotes like "these"!]
	end
	
Okay, last one! This literal actually has a little more meat to it since it's a data type we haven't covered in depth yet: ranges. Ruby has the concept of a range of values; given a start and an end point, Ruby will fill in the blanks. It looks like this:

	<!--
	  meat (содержание; суть;)
	  since (постольку поскольку;)
	  range (диапазон; ряд;)
	  concept (методология; подход; концепция;идея; понятие;)
	  blank (пропуск;)
	-->


	ranger_smith = 55..75
	puts "ranger smith: #{ranger_smith}"

	start = 101
	finish = 201
	ranger_rick = start..finish
	puts "ranger rick: #{ranger_rick}"

	smith_points = ranger_smith.map {|n| n.to_s }
	puts "all together now! #{smith_points.join(", ")}"

Here, we see a few interesting properties of ranges. First off, ranger_rick is defined using variables embedded in the Range literal. This makes sense when we think about those two little dots as being no different from the square brackets we use in arrays... but it's not immediately obvious that this is allowed. Next, we see that a range is a collection. We can map over the range to transform it into an array containing all the points explicitly. Range includes Enumerable, which gives us access to all its elements.

Now try a couple for yourself. Discover the syntax for the "one less" literal (which excludes the latter element) and for the range-of-characters literal. Note: I won't be using Range to test if you got the right answer, because that would print the literal for you. The tests will compare ranges mapped into arrays.

	<!--
	  first off (для начала; сначала;)
	  to embed (вставлять; внедрять;)
	  obvious (очевидно; видно; ясно;)
	  map over (установить соответствие; отобразить;)  
	  point (точки;)
	  explicitly (явно; конкретно; ясно;)
	  latter (последнее; последний из двух;)
	  map into (отображать в;)
	-->

	def one_less
	  1...10
	end

	def range_of_characters
	  'a'..'z'
	end

There are still more literals in Ruby for you to explore, though these are the most common. We'll take a look at these more advanced literals later on when we cover the topics they govern. Before you leave this chapter, experiment on your own to see if you can create negative ranges!

	<!--
	govern (определять;)
	-->
