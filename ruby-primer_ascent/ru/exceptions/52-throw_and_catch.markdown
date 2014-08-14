# 5.2 Throw and Catch #

## Why not 'raise' and 'rescue'? ##

At some point every apprentice is tempted to look for a quick escape from a self-painted corner. The apprentice sees the trap of her own making at the bottom of many nested methods or loops. "I know!" she thinks. "I'll jump out with an exception!"

But therein lies danger: exceptions are meant to be quite literal. One should raise and exception under exceptional circumstances, and only then. The temptation to use exceptions to move about the code is one we all experience; don't worry. But that doesn't make it any less wrong. "Moving about the code" is known in fancy circles as "flow control." Your tools for flow control include if (and its friends, unless and else), Enumerable#each, for, method calls, and the like.

The reason not to use exceptions for flow control isn't simply one of semantics, either. Exceptions are designed to provide you, the programmer, with as much information as possible about what went wrong (since they imply a failure). To do this, Ruby's exception system figures out all the methods you called to get where you are -- and this is slow. Control flow code should be fast. Exceptions are slow. One more reason to do things correctly.

...and how do we do things correctly? In the Ruby world, throw and catch are an option. Of course, "doing things correctly" is always philosophical and subjective -- but we can pontificate a bit more after a few examples.

	<!--
	  apprentice (ученик; подмастерье; новичок;)
	  tempt (соблазнять; уговаривать; склонять; искушать; заманивать;хотеться;)
	  paint yourself into a corner (загнать себя в угол; создать себе сложную ситуацию;)
	  quick escape (быстрый побег)
	  self-painted (самим раскрашенный)
	  trap (капкан; западня;ловушка;)
	  jump out (выскочить; выскакивать;)
          therein lies (в этом заключается;)
	  quite (вполне;довольно; совершенно;в некоторой степени;)
	  literal ()
	  circumstance (обстоятельство;обстоятельства; среда; условаия; случай;)
          temptation (соблазн; искушение; обольщение;)
	  move about (переезжать; переносить с места на место; перейти;)
	  fancy ()
	  
	-->

## Jump out! ##

Let's start with the canonical throw / catch example: breaking out of nested loops. Imagine you are writing a robot whose job it is to blindly search a floor for a piece of candy. The following code will work:

	floor = [["blank", "blank", "blank"],
	         ["gummy", "blank", "blank"],
	         ["blank", "blank", "blank"]]

	candy = nil
	attempts = 0
	floor.each do |row|
	  row.each do |tile|
	    attempts += 1
	    candy = tile if tile == "jawbreaker" || tile == "gummy"
	  end
	end
	puts candy
	puts attempts

Notice that even though we found a gummy on the fourth tile, we still checked all the remaining tiles, totally nine attempts.

throw and catch can save us this extra work by "jumping out" of the two loops. This is the canonical example because it's easy to see that a simple break statement would only escape the inner loop.

	Example Code:

	floor = [["blank", "blank", "blank"],
	         ["gummy", "blank", "blank"],
	         ["blank", "blank", "blank"]]
 
	attempts = 0
	candy = nil
	catch(:found) do
	    floor.each do |row|
	    row.each do |tile|
	      attempts += 1
	        if tile == "jawbreaker" || tile == "gummy"
	        candy = tile
	        throw(:found)
	      end
	    end
	    end
	end
	puts candy
	puts attempts
 


There are a few interesting points to make about the throw / catch code:

* We tell catch what symbol it should capture from the throw (in this case, :found. These need to match.
* throw works much like return, in that no code after it is ever executed.
* throw requires a catch to pair with it. Try removing the catch and see what happens!



## Jump out with a payload ##

There's one more feature of catch that we can exploit to make this code a little more concise. throw accepts both an identifying symbol and a return value; the matching catch will return this value. Notice that this allows us to declare candy directly.

	Example Code:

	floor = [["blank", "blank", "blank"],
	         ["gummy", "blank", "blank"],
	         ["blank", "blank", "blank"]]
 
	attempts = 0
	candy = catch(:found) do
	    floor.each do |row|
	    row.each do |tile|
	      attempts += 1
	        throw(:found, tile) if tile == "jawbreaker" || tile == "gummy"
	    end
	    end
	end
	puts candy
	puts attempts

## Consider the alternatives ##

As always, there's more than one way to pet a cat. But sometimes you'll pet the cat in a way he doesn't like and an arm covered in scratches is your reward. throw and catch are not common in Ruby code. More often than not, there's a solution which is not only simpler but easier to follow. One such solution, which mimics the behaviour we saw in the last example, is to use the return value of a function in place of the return value of a catch. The throw approach expresses more intent than exceptions, as discussed earlier. The approach of using a function expresses even more intent because we're forcing ourselves to give this operation a name.

Change the last example to return the found tile from a method called search, instead. search should receive the floor plan as a parameter.

 
	candy = catch(:found) do
	    floor.each do |row|
	    row.each do |tile|
	        throw(:found, tile) if tile == "jawbreaker" || tile == "gummy"
	    end
	    end
	end
	puts candy
 
