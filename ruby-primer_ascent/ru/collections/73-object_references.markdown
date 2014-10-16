# 7.3 Object References

## A different perspective on enumerables

Enumerables are collections of objects. Implict in that definition are two concepts: object references and collection ordering.

Ruby, like most other object oriented languages, deals with references to objects rather than the objects themselves directly. What does that mean?

    <!--
      concept (понятие; идея; подход; концепция;)
    -->

    a = "tom"
    b = "jerry"
    superheroes = [a,b]
    a = "batman"

    puts superheroes
    
What insight did you infer from this simplistic example? Yes, Batman is not a superhero!

But what else? Let me test your comprehension with an exercise. Figure out a way to change the value of b to "batman" so that printing the contents of superheroes lists tom and batman.

    <!--
      insight (понимание;)
      infer (заключать; делать заключение; вывод;)
      simplistic (простенький;)
      comprehension (понимание;)
      figure out (выяснить;)
      
    -->

    a = "tom"
    b = "jerry"
    superheroes = [a,b]

    b.sub!("jerry","batman")
    puts superheroes
    
Let us talk about String#sub! a bit. You might have observed the bang (exclamation symbol: !) in the sub! method. It typically means 'dangerous' in Ruby. It is dangerous because it mutates the string in-place. Lets see how:

    <!--
      mutate (видоизменять; трансформироваться;)
    -->

    a = "tom"
    puts "#{a}, #{a.object_id}"
    a = "jerry"
    puts "#{a}, #{a.object_id}"
    
Now compare the output of the example above with that of this:

    a = "tom"
    puts "#{a}, #{a.object_id}"
    a.gsub!("tom", "jerry")
    puts "#{a}, #{a.object_id}"
    
In the first example, we made the variable <b>a</b> refer to a different object (the string "jerry"). Thus the object_id, which is the id of the object that the variable refers to, changed.

However, when we did gsub! on a, the object_id remained the same, but the underlying object's value changed. This is what is referred to as 'mutating' an object. Let us see what impact this has when dealing with Enumerables.

    <!--
      impact (воздействие;)
    -->  

    a = "tom"
    b = "jerry"
    superheroes = [a,b]
    puts superheroes

    # reassign a to a different superhero
    a = "batman"
    puts superheroes

    # jerry is in fact superman. who knew!
    b.gsub!("jerry", "superman")
    puts superheroes
    
This is why bang methods are dangerous - they mutate the objects in-place. Your Enumerable is only a collection of 'references' to objects. This is good - even if you change what a variable refers to, later in the code, the Enumerable will not change. After all, the Enumerable does not know anything about the variable. It stores only the objects referenced by those variables. But when you use a dangerous method, you are changing the value of every Enumerable that contains the object. You rarely want that.    

    <!--
      bang (восклицательный знак)
      rarely (крайне редко)
    -->
