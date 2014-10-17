# 8.1 The Debugging Primaries

## puts and p

There are many kinds of bugs and consequently many ways to debug them. In this chapter we'll teach you some simple techniques you can use to isolate the bugs and understand where and why they are happening.

We'll start with the trusted p and puts. We've already discussed them in the RubyMonk Ascent: More Classes chapter. Both puts and p serve similar purposes, but are slightly different. Typically p is handy because it prints the output of inspecting the object which is more detailed than what puts provides. For the sake of brevity, I'll just refer to p in places where either puts or p makes sense.

Here is an exercise to demonstrate the use of p. I've implemented a simple method that returns the name and age that you pass to it. But it is somehow not working. I suspect that the method expects data in a format different from what I'm sending it. You have to fix the method so that it prints the name and age from the parameter I pass to it.

    <!--
      isolate (изолировать; отделять; обособлять;)
      trusted (надежный;)
      serve (служить;)
      inspecting (проводить осмотр; внимательно осматривать;)
      for the sake of brevity (для краткости;)
      somehow (как-нибудь; каким-то образом;)
      suspect (сомневаться;)
    -->
    
    def describe(user_info)
      "My name is #{user_info["name"]} and I'm #{user_info["age"]} years old."
    end    
    
That was easy, wasn't it?

Since we're on the topic of bugs, let me demonstrate a common bug the uninitiated is prone to make: dividing integers and expecting a float. Example time:

    <!-- 
      topic (тема; раздел;)
      uninitiated (непосвященный; несведующий; неискушенный;)
      prone (склонный;)
    -->  

    p 10/2
    p 10/3
    p 10.to_f / 3
    
As you can see, dividing 10 by 2 gives 5, the correct result. The dividend, divisor and quotient here are all integers. But what about 10/3 - it should return 3.3333333333333335 but instead, it returned 3, the integer part of the result. That is not what we typically expect - if you divide two numbers, you need the correct result - not just the integer part of the division.

This happens because Ruby converts the result to an integer - even if the result is not strictly an integer. Lets see how we can solve this using the Float datatype.

    <!--
      dividend (делимое;)
      divisor (делитель;)
      quotient (частное; результат деления;)
      typically (как правило; чаще всего; в основном;)
      strictly (строго; определенно;)
    -->

    p 10.class
    p (10.0).class

    p (10.0/3)
    

What did you learn from the above example? Integers in Ruby are of class Fixnum, and numbers with decimals (which we call 'floating point numbers') are Float. And of course, if you divide two numbers and expect the correct result with decimals, then at least one number in the operation must be a Float.

Now, let's look at a slightly elaborate exercise. I've implemented the class DrivingLicenseAuthority that decides whether to grant a license to applicants or not. But unfortunately, it is buggy. I'm not going to tell you all the details so you'll have to find out where things are breaking and why - the tests are pretty bland. The goal is to get all the tests to pass. Remember, p is your friend.

Blindfolded, you may now debug this code!    

    <!--
      decimal (десятичная дробь; дробь; десятичная точка;)
      floating point number (число с плавающей точкой;)
      at least (как минимум;)
      elaborate (сложный;)
      grant a license (выдавать лицензию;)
      applicant (лицо; кандидат; заявитель; проситель;)
      unfortunately (к сожалению; как ни печально;)
      buggy (содержащий ошибки;)
      find out (выявить; найти; отыскать;)
      bland (вежливый;ласковый; мягкий; успокаивающий(лекарство))
      blindfolded (с завязанными глазами;)
      visual acuity (острота зрения;)
    -->
    
    class VisualAcuity
      def initialize(subject, normal)
        @subject = subject
        @normal = normal    
      end
      def can_drive?
        (@subject.to_f / @normal.to_f) >= 0.5
      end  
    end

    class DrivingLicenseAuthority
      def initialize(name, age, visual_acuity)
        @name = name
        @age = age
        @visual_acuity = visual_acuity
      end
  
      def valid_for_license?
        @age >= 18 and @visual_acuity.can_drive?
      end
  
      def verdict
        if valid_for_license?
	        "#{@name} can be granted driving license"
        else
          "#{@name} cannot be granted driving license"
        end
      end
    end    
