# 5.0 Handling and Raising #

## What is an exception? ##

As I'm sure you've seen in RubyMonk already, it is possible for your code to generate errors, known as exceptions. When an exception occurs (Ruby calls this even "raising an exception"), it will stop processing further statements in the code and will try to escape any methods called so far. This description is a little difficult to visualize, so let's look at an example:

	<!--
	  to raise (вызывать;)
	  to process (обрабатывать;)
	  to escape (избегать;)
	-->

	def extract_first_name(name)
	  first = name.split(" ").first
	  puts "extracted first name #{first}"
	  first
	end

	def casual_names(names)
	  casual = names.map {|name| extract_first_name(name) }
	  puts "captured casual names #{casual}"
	  casual
	end

	puts casual_names(["Yehuda Katz", nil, "Why the lucky stiff"])
	puts "I bet you'll never see this."

Notice that the code does extract Yehuda's name before it fails. But once the failure (attempting to call #split on nil) occurs, the program stops and further statements aren't processed. This is the first thing to do know about exceptions:
	
	<!--
	  to extract (получать; извлечь;)
	  to attempt (пытаться; пробовать;)
	  to halt (останавливать; обрывать;)
	-->

* Raising an exception halts program execution.

Are you asking yourself, "So... if program execution doesn't continue forward... where does it go?" Of cource you are. The visualization of the program's execution point making a quick escape is quite close to the reality. It might add clarity to that image to know that the raised exception makes its way through all the methods called (known as the "call stack") to get to the point of failure. In the example above, the failure in extract_first_name then propagates backward through casual_names and finally back up to the "top", where we called casual_names with an array. This brings us to the second thing to know about exceptions:

* A raised exception will propogate through each method in the call stack untill it is stopped or reaches the point where the program started

This of course implies that exceptions can be stopped. Which brings us to our next section.

	<!--
	  forward (дальше; вперед;далее;)
	  quite close (достаточно близко;)
	  clarity (наглядность;)
	  point of failure (место ошибки;)
	  to propagate (передаваться через среду;)
	  backward (назад; обратный)
	  back up (давать задний ход;)
	  imply (означать; подразумевать; предполагать; значить;)

## begin, rescue, end ##

To stop and exception, we need a way of wrapping our method calls such that they are protected when an exception pops out of them. Behold! The begin - rescue - end block!

	def start_summer
	  raise Exception.new("overheated!")
	end

	begin
	  start_summer
	rescue Exception
	  puts "It's cool in here, boy. For the rest of the summer, we'll live in the refrigirator."
	end

Notice that the exception we raised with raise (covered further at the end of this lesson), was created using Exception.new. That's because Exception is just a Ruby class, like any other. It stands to reason, then, that our final defining characteristic of exceptions is:

* Exceptions are just Ruby objects.

	<!--
	  pop out (выскочить;)
	  to behold (заметить; видеть; замечать;смотреть;)
	  it stands to reason (само собой разумеется; очевидно; понятно; ясно; разумеется;)
	  to rescue (спасать; избавлять; освобождать;)
	  conveniently (в целях удобства;)
	-->

You may have also noticed we used the Exception class name when we rescued our exception to stop it in its tracks. We can conveniently give our exception instance a name by extending this syntax:

	def start_summer
	  raise Exception.new("overheated!")
	end

	begin
	  start_summer
	rescue Exception => e
	  puts "Let me tell you about heat! #{e.inspect}"
	end

Cool. Now try one yourself, but we'll change it up a bit. If you define a begin-rescue-end block inside a method it actually doesn't require the begin and end keywords; as long as you want to capture exceptions across the entire method, they're implicit.
Knowing that, write a method called decode_all which takes an array of strings as a parameter. decode_all should call decode (the world's worst encryption algorithm) with each string, rescuing on Exception. decode will record each secret uncovered so far using announce.
	<!--
	  as long as (поскольку; если только; пока; до тех пор, пока;)
	  across (по;)
	  implicit (неявный; подразумеваемый)
	  worst (наихудший;)
	  rescue on (спасать от;)
	  to announce (оповещать;извещать;)
	  to explode (взорваться; взрывать; взрываться;)
	-->

	EXAMPLE_SECRETS = ["het", "keca", "si", nil, "iel"]

	def decode(jumble)
	  secret = jumble.split("").rotate(3).join("")
	  announce(secret)
	  secret
	end

	def decode_all(secrets)
	  secrets.map {|s| decode(s) }
	rescue
	  puts "whew! safe."
	end

Right on! It always feels good to rescue someone. Let's try it one last way. rescue has a one-liner form which allows you to rescue the exception at the end of the line (similar to the if one-liner). This form, however, simply returns whatever is placed to the right of the rescue keyword. Since we're not doing anything with the exception object yet anyway, that sounds like an acceptable way to save our program from crashing. Rescue with the string "it's okay, little buddy." if an exception is raised:

	EXAMPLE_SECRETS = ["het", "keca", "si", nil, "iel"]

	def decode(jumble)
	  secret = jumble.split("").rotate(3).join("")
	  announce(secret)
	  secret
	end

	def decode_all(secrets)
	  secrets.map {|s| decode(s) } rescue "it's okay, little buddy."
	end

	<!--
	  Right on! (Точно!)
	  acceptable (приемлевый; подходящий; допустимый;)    
	-->


## ensure ##

The last aspect of rescuing exceptions is forcing execution of some cleanup code you need to run whether there was an error or not. This sort of thing often involves closing a connection to a database of freeing up some other non-Ruby resouce. Doing so is quite easy: we use the ensure keyword, at the same indentation level as rescue, to define code that should run both in success and failure scenarios.

Try it yourself. Imagine we have a connection to a database which we can close with Database#close. Clean up your database connection in the following exercise.

	<!--
	  to force (принудительно выполнить;)
	  clean up (очищать;)
	  whether (несмотря на то, что;)
	  indentation (отступ;)
	-->

	class UserDataAccess
	  attr_accessor :db

	  def initialize
	    @db = Database.new
	  end

	  def find_user(name)
	    @db.find("SELECT * FROM USERS WHERE NAME = '%'", name)
	rescue Exception => e
	  puts "A database error occured."
        ensure 
	  @db.close
	  end
	end


## raise ##

To raise exceptions we use raise or Kernel.raise or Kernel.fail. raise without any arguments simply raises a RuntimeError exception with the string message that we've specified.

	raise "Rise."

Although Ruby does raise exceptions it can handle, on its own, you would typically use raise when you want to raise these exceptions before Ruby does, or raise custom exceptions that are not handled by Ruby.

These would commonly be StandardError exceptions that we explain in the next lesson. This example raises a ZeroDivisionError on its own, which is an exception sub-classes by StandardError.

	42 / 0

The most common way ot raise an error is to just specify the type of the exception with an optional string parameter that acts as its message when it is displayd.
	
	raise StandardError, "Raising standard error"

Write a string_slice method that accepts 5 string parameters and raises ArgumentError if more than 5 are passed in. string_slice returns a sequential array of these strings sliced up until the third character; it alse raises IndexError if the string is less than 3 characters long.

	<!--
	  to handle (обрабатывать;)
	  on its own (сам по себе; собственными силами;)
	  typically (как правило;)
	  custom (специальный;)
	  commonly (обычно; часто;)
	  common way (распространенный способ;)
	  optional (дополнительный; необязательный;)
	  slice up (разделить на части;)
	-->

	def string_slice(*params)
	  raise ArgumentError,"More then 5 parameters" if params.count>5
	  params.map do |param| 
	    if param.length < 3
	      raise IndexError, "Less then 3 character long"	
	    else
	      param[0..2] or param.slice(0..2)
	  end
	end

