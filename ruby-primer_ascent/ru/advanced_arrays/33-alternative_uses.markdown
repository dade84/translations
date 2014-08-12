# 3.3 Alternative Uses #

## Disjoint Arrays ##

So far, we've behaved. All our arrays have been clean, containing only objects of the same type.(String, Integer, etc.) However, we can abuse the fact that arrays permit disjoint types. "Disjoint types" are types which are grouped together (in out case, in an array) but which are not all of the same class. Before we even get started, a word of caution: though possible, one rarely has reason to put different types of objects in an array. Let's leave the discussion of when not to use this technique for after.

	<!--
	  so far ()
	-->


FIrst off, let's have a quick look at just how disjoint an array can be:

	["xkcd", 9, Object.new, ["a", "clean", "array"], {:a => "hash"}].each do |e| puts e.class
	end

Now, let's use Ruby's flexible, dynamic nature to solve a fake real-world problem. You are building a robot to take orders at The Tasty Gem Sandwich Shop. Any order placed at the shop can only account for one sandwich and one drink. (Don't ask me. It's been this way since 1954 when it opened.) Fortunately, the sandwich chef robot takes these orders in a sensible format: a hash. Less fortunate is the output he returns to each order robot: for some reason, he flattens the order hash into an array when he announces that an order is ready. This will cause us some trouble when we try look up which table we need to serve.

Fill out the template for your robot below. A customer will call the place_order method, which you'll have to pass on to the chef robot with your robot's name. The chef will then call your robot's serve method when the customer's sandwich is ready. Your robot needs to serve the correct sandwich/drink combo to the correct table at that point from the chef's sloppy data format.

	class WaiterRobot
	  def initialize(chef, tables)
	    @chef = chef
	

As you can see, disjoint arrays are really not that clean. However, they are a really someone else will present to you at some point. In this case, it was our beloved chef. But one day it may be the API of the social network de jour or a frient you're building something with. Knowing how to transform ugly data (a disjoint array) into pretty data (a hash) will alleviate much of the pain the former brings.

## Fake matrix ##

"Fake Matrix": what happens when you take the red pill on Groundhog Day and repeat the day in the real world. When presented with the pills again in the real world you take the bule pill out of frustration -- this time to remain in blissful ignorance... yet trapped in reality. Wait, what?

Nah. Not that. Fake matrices. If you receive an array in the shape of a matrix, you can do some basic matrix math on it. Even if you don't think of them a matrices per se, sometimes these operations will still come in handy. Imagine you want to create a list with all the combinations of couples between the various characters in Degrassi Junior High. You could type them all out by hand, or write a method to come up with the combinations for you, but thankfully RUby already has one built in: Array#product. Try it out. Let's assume we're not restricting couples in any way -- the only is no one can create a couple by him/herself.

	CHARACTERS = ["Joey Jeremiah", "Snake Simpson", "Wheels", "Spike Nelson", "Arthur Kobalewscuy", "Caitlin Ryan", "Shane McKay", "Rick Munro", "Stephanie Kaye"]

	def degrassi_couples
	end

You may have noticed there is one problem with this solution: the couples themselves aren't unique. ["Joey Jeremiah", "Spike Nelson"] and ["Spike Nelson", "Joey Jeremiah"] are not different couples as far as we're concerned, but Ruby doesn't know this. How do you think you could remove these duplicates? Try out your ideas in the exercise above. We'll cover uniqueness in upcoming "Collections" chapter.

There's one other pseudo-matrix operation that comes in handy when you have data which is coincidentally shaped like a matrix: Array#transpose. Transposing a matrix is just rotating it on an axis. Use this transformation to pass an array to a row-based printer. By "row-based", we mean only two elements per child array. The array you'll receive from the caller will be column-based. By "column-based", we mean there are only two child arrays, each with N elements -- where N is the number of rows.

	ex

With all that said, of course, if you actually require true matrix operations please use the Matrix class! It's got all sorts of powerful math power.
