http://4loc.wordpress.com/2009/01/16/the-splat-operator-in-ruby/

# The splat operator in Ruby #

The splat operator in Ruby, Groovy and Perl allows
you to switch between parameters and arrays:
it splits a list in a series of parameters,
or collects a series of parameters to fill an array.

The split mode turns an array into multiple-arguments:

	>> duck, cow, pig = *["quack","mooh","oing"]
	=> ["quack","mooh","oing"]

The collect mode turns multiple-arguments in an array:

	>> *farm = duck, cow, pig
	=> ["quack","mooh","oing"]
	
The splat operator can be used in a case statement :

	WILD = ['lion','gnu']
	TAME = ['cat','cow']
        
        case animal
	  when *WILD
	  "Run"
	  when *TAME
	  "Catch"
	end

And it can be used to convert a hash into an array:

	>> a = *{:a=>1,:b=>2}
	=> [[:a,1],[:b,2]]
	>> Hash[*a.flatten]
	=> {:a=>1,:b=>2}


