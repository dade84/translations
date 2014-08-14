# 5.1 Tidying Things Up #

## Capture and control flow ##

Exceptions are simply, just packaged objects that contain error information. These are typically meant for more exceptional cases that require halting the current program flow to handle abnormal conditions. This is different from else clauses, error codes or state variables which are more about guiding the control flow.

They also allow you to clearly segregate the expected processing of your workflow from the anticipated exceptions cases in your code

	begin
	  eval "40 / 0"
	rescue ZeroDivisionError => error
	  p error
	end

This is exactly what we want. It does a few important things.

	<!--
	  tide up (убирать; убрать;)
	  control flow (поток управления; последовательность управляющих команд;)
	  typically (как правило; чаще всего;)
	  to mean (предназначать; предназначить;)
	  exceptional (исключительный; необычный; особенный;)
	  to halt (останавливать; обрывать; прекращать;)
	  abnormal (неправильный; неработоспособный;)
	  be about (ориентироваться на;)
          to guide (вести; направлять;)
	  segregate (отделять; выделять; изолировать; отделяться;)
	  anticipated (предполагаемый; прогнозируемый;)
	-->

Our code in neatly segregated from the part where the exception is handled. The exception is handled after the rescue clause and everything before that in the begin block is code that has problems that we can anticipate.

	<!--
	  neatly (аккуратно; четко;)
	  backtrace (отслеживать;обратная трассировка;след;)
	-->

In this next example, we call the backtrace method on the error variable, which is an instance of the SyntaxError class. This variable allows us to go through the current call stack of your program.

	def zen
	  begin 
	    p eval "(40 + 2) / 2"
	    p eval "(40 + 2) \ 2"
	  rescue SyntaxError => error
	    puts error.backtrace
	  end
	end

	zen

Another important thing it allows us to do is halt execution. Notice how it exits the block after the second eval line raises an exception. Nothing else is executed after it.

	def zen
	  begin
	    p eval "(40 + 2) / 2"
	    p eval "(40 + 2) \ 2"
	    p eval "4, 8, 15, 16, 23, 42"
	  rescue SyntaxError => error
	    puts error.backtrace
	  end
	end

	zen


This is one of the differences between raising error codes and raising exceptions. Exceptions preempt the usual workflow, error codes are more passive, and explicitly need to be handled later on in your program.

Here's what an error code handling flow might look like, handling zen? that returns 200 in the if block sometime after.

	def zen?(object)
	  if object.to_i == 42
	    200
	  else
	    400
	  end
	end

	if zen?(42) == 200
	  puts "Master loves you as he loves Jacob."
	end

Error codes are also specific to the domain of your problem. Exceptions, on the other hand, by default, are more tightly coupled with the Ruby API. It is, however, possible to create custom exceptions, other than the ones already defined by the Exception class.

	<!--
	  preempt (упредить;сделать что-то раньше;)
	  usual (нормальный;)
	  specific (особый; специальный; конкретный;)
	  domain (область;)
	  tightly coupled (сильно связанный;тесно связанный;)
	  custom (специальный; свой; собственный;)
	  other than the ones (кроме тех, которые)
	-->

## Propogation ##

Exceptions always bubble up towards the relevant exception handler. Imagine as if you were trying to chase a ghost in a haunted house running across a chain of rooms. We go outwards in terms of how our blocks are scoped we're technically going deeper into our call stack.

	def zen
	  10.times do
	    answer = 42 / 0
	  end
	end

	begin
	  puts "Calling zen."
	  zen
	rescue ZeroDivisionError => error
	  puts "Rescued from the zen method."
	  puts error.backtrace
	end

	puts "End of main."

Notice how the division by zero exception propogates outwards from the 10.times block -> to the zen methods -> to the rescue ZeroDivisionError clause outside the method. The backtrace is then printed after the exception is rescued, and any further execution is stopped.

	<!--
	  propogation (распространение;)
	  bubble up (быть переданным/перенаправленным)
	  relevant (важный; имеющий отношение)
	  to chase (гнаться; преследовать; охотиться;)
	  haunted house (дом с привидениями;)
	  outwards (наружу; за пределы;)
	  in terms of (относительно; говоря языком; в контексте; в части, касающейся;)
	  to propogate (распространять; передавать;)
	  hence (следовательно;)
	-->

What happend when you don't handle the exception anywhere?

	def zen
	  10.times do
	    answer = 42 / 0
	  end
	end

	begin
	  puts "Calling zen."
	  zen
	end

	puts "End of main."

Just like before, the line where answer is being assigned, an exception is raised. This exception propogates further up the call stack. The caller of the 10.times block is zen for which the caller is the begin block. At this point, the exception is not explicitly rescued anywhere, hence it is just printed onto the STDERR and the program exits.


## Hierarchy and the Exception class ##

All the error objects that we've been using come from the Exception class. These objects are typically created by raise or by simply calling new on the exception object.

The Exception is the master class that subclasses a bunch of more granular exceptions.

	p StandardError.ancestors
	p ZeroDivisionError.ancestors
	p SyntaxError.ancestors

The larger part of the commonly recoverable exceptions are sub-classes by the StandardError class. The others are more low, virtual-machine level errors that are less important in a reqular course of the program and are generally not captured.

	<!--
	  master (главный; основной;)
	  granular  (детальный; детализированный;)	
	  commonly ( обычно; в большинстве случаев;)
	  recoverable (допускающий повторное использование;) 
	  regular course (обычный курс)
	  
	-->

This is the StandardError class hierarchy:

	ArgumentError
	IOError
	  EOFError
	IndexError
	LocalJumpError
	NameError
	  NoMethodError
	RangeError
	  FloatDomainError
	RegexpError
	RuntimeError
	SecurityError
	SystemCallError
	SystemStackError
	ThreadError
	TypeError
	ZeroDivisionError

For more granularity in your exceptions, you can rescue each of these individually. Rescueing StandardError takes care of all the sub-classed exceptions as well. In this example, we rescue ZeroDivisionError by rescuing StandardError.

	begin
	  eval "40 / 0"
	rescue StandardError => error
	  p error
	end

Therefore, to create a custom Exception, you can subclass the Exception class. More specifically, the StandardError class.

	class SomeCustomError < StandardError
	end

It is generally a good practice to end your custom exception name with Error.

backtrace method on the Exception class returns an array of the current state of the call stack. Starting from the first element, it gives you strings of errors where the exception originated from, till it reaches the end of the call stack.

message returns a string that is specified when you create or raise the exception object. It returns the name of the exception if no message is specified.

	class InfinityError < StandardError
	end

	ie = InfinityError.new("infinity error was raised")

	begin
	  if 1.0 / 0.0
	    raise ie
	  end
	rescue InfinityError => error
	  p error.message
	end

Raise a custom exception handler KasayaError in the robe method if the argument type is not a String "Kasaya". It should return "Dharmaguptaka's Kasaya Robe" otherwise.

	<!--
	  therefore (по этой причине; следовательно; потому; оттого; таким образом;)
	  more specifically (точнее говоря)
	  originate from (возникать из; происходить из; )
	-->

	class KasayaError < StandardError
	end

	def robe(type)
	  unless type.downcase == "kasaya"
	    raise KasayaError, "Wrong robe!"
	  end
  
	  "Dharmaguptaka's " + type.capitalize + " Robe"
	end
