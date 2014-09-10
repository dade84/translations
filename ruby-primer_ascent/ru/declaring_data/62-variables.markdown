# 6.2 Variables #

## Scope ##

We've seen many variables already in rubymonk. However, we haven't much discussed how they vary in terms of visibilty. Visibility, or scope, is what differentiates kinds of variables (local, global, instance, and class) but also determines where a variable can be accessed from. Let's have a look.

	<!--
	  vary (отличаться;)
	  in terms of (если говорить о; по типу; относительно;)
	  differentiate (отличать; различать; вызывать расхождение;)
	-->

	def scope_the_scope
	  on_the_inside = "oh. hi, friends."
	  puts "from the inside: #{defined?(on_the_inside).inspect}"
	end

	scope_the_scope
	puts "from the outside: #{defined?(on_the_inside).inspect}"

While it may seem obvious at this point that on_the_inside is not defined outside the method scope_the_scope, it may not be entirely obvious why. Take a moment to consider how the Ruby interpreter is reading your code. If you remove the call to scope_the_scope in the above example, Ruby still reads the puts line... but it doesn't execute. Why?

Every time the Ruby interpreter encounters def, it enters a new scope. This means simultaneously that it is capturing the code its about to read into a name (in this case, `scope_the_scope`) but also that it is creating a new context in which that code will be read. That context is the local scope, and it cuts both ways: variables defined inside the method cannot be changed or read outside the method -- variables defined outside the method cannot be changed or read inside the method.

	<!--
	  obvious (явный; очевидный; заметный; ясный;)
	  take a moment to (уделить минуту;)
	  consider (рассматривать; рассмотреть; почитать; анализировать;)
	  encounter (сталкиваться; наталкиваться;)
	  simultaneously (одновременно; совместно; в то же самое время;)
	  be about (предполагать; иметь отношение; собираться; )
	  read into (передавать;)
	  it cuts both ways (палка о двух концах;)
	  otherwise (в противном случае; иначе;)	
	-->

Notice our use of Ruby's keyword defined?. This is a special keyword in Ruby (like if), not a method, because we would otherwise see NameError for undefined variables. The same concept applies to instance variables, with an extended example:

	class Shoe
	  def initialize(toes = 1)
	    @toes = toes
	  end
  
	  puts "inside the class: #{defined?(@toes).inspect}"
  
	  def i_can_haz_toes
	    puts "inside the instance: #{defined?(@toes).inspect}"
	  end
	end

	class Foot
	  def initialize(toes = 5)
	    @toes = toes
 	end

	  puts "inside the class: #{defined?(@toes).inspect}"
  
	  def i_can_haz_toes
	    puts "inside the instance: #{defined?(@toes).inspect}"
	  end
	end

	samurai_boot = Shoe.new(2)
	samurai_boot.i_can_haz_toes

	left = Foot.new
	left.i_can_haz_toes

	puts "in the `main` class: #{defined?(@toes).inspect}"

As you can see, defined? doesn't just report whether the variable is defined but also how it is scoped.

Scope will become an important topic when you start metaprogramming. When you read "Metaprogramming Ruby" and "Metaprogramming Ruby: Ascent" scope will come up frequently. The yin to scope's yang is binding, which is the Ruby terminology for the context we previously mentioned when discussing method definitions.

	<!--
	  whether (ли;)
	  topic (тема;)
	  come up (подниматься; возникать;)
	  yin (инь)
	  yang (янь)
	  mentione (упоминать;)
	-->

## Globals ##

If Ruby is your first programming language (nice choice!), you probably haven't heard about "global" variables yet. If you're coming to Ruby from another language, you have no doubt been warned about the dangers of globals. Or perhaps you have experienced them for yourself. Hmm. That doesn't make them sound very appealing, does it? Well, generally they're not.

So why would we bother with globals at all? First off, it's important to know that they exist. You may read someone else's Ruby code one day and wonder "what the heck is this thing prefixed with a dollar sign?" You may also find yourself needing one of the pre-defined globals -- or finding it more convenient in a short, throw-away Ruby script. More on those in a moment.

	<!--
	  doubt (сомнение;)  
	  appeal (привлекательность;)
	  generally (как правило;)
    	  bother (беспокойство;)
	  throw-away (временно используемый; пробный;)	
	-->

	module Somewhere
	  class Hidden
	    $everywhere = "global love"
	  end
	end

	module Somewhere
	  class CloseTo
	    def the_heart
	      $everywhere
	    end
	  end
	end

	puts Somewhere::CloseTo.new.the_heart

As you can see, a global has its own beauty in its simplicity: you can set it anywhere, you can get it anywhere. There is only one global with that name, no matter where you go. However, this is also the global's undoing. Because everyone can see a global, everyone can change it. You have no special control. Someone working on the same codebase might change the global in a way you weren't expecting and your code will be confused by the new value. Or perhaps you're working solo on a project and you forget that you were setting the same global way in that file over there even though today you're working way over here in this file. A program is much easier to reason about in small pieces -- globals make the pieces as big as possible!

Ruby does have some predefined globals which you may make use of from time to time. These include $* (the command-line arguments used to execute this Ruby program), $@ (the location of the last error), $~ (the last regular expression match), and $0 (the name of the current ruby script). To name a few. As you can see, these predefined globals are set by the Ruby interpreter itself and usually refer to "the last such-and-such that happened". Sometimes this is convenient. Usually there is a more explicit way to gather this information. So you can say you've used them, fill in the expected predefined globals here:

	<!--
	  beauty (красота;)
          simplicity (простота;)
   	  no matter where(где бы ни было;)
 	  undoing (уничтожение;)
	  solo (сделать что-то в одиночку;)
	  reason about (рассуждать;)
	  such-and-such (такой-то;)
	-->

	def last_error
	  $@
	end

	def this_script_name
	  $0
	end

If you experimented, you may have noticed that some predefined globals such as $* and $$ either throw security exceptions or simply return nil. This is because code in rubymonk runs in an a sandboxed environment and access to these variables is restricted for security reasons. Sorry about that!
	
