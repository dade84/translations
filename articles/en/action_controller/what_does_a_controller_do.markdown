# What does a controller do? #

Action Controller is the C in MVC. After routing has determined which controller to use for a request, your controller is responsible for making sense of the request and producing the appropriate output. Luckily, Action Controller does most of the groundwork for you and uses smart conventions to make this as straigth-forward as possible.

For most conventional RESTful applications, the controller will receive the request (this is invisible to you as the developer), fetch or save data from a model and use a view to create HTML output. If your controller needs to do things a little differently, that's not a problem, this is just the most common way for a controller to work.

A controller can thus be thought of as a middle man between models and views. It makes the model data available to the view so it can display it to the user, and it saves or updates data from the user to the model.

	<!--
	  to determine (определять; устанавливать; )
	  responsible (обязанный; отвественный; несущий ответственность;)
	  make sense of (разобраться в; понять смысл;)
	  luckily ( к счастью; )
	  most of (большая часть;)
	  groundwork (подготовительная работа;)
	  stright-forward (простой; прямой; открытый; ясный; честный;)
          fetch (извлекать; получать; вызывать;)
	  most common way (наиболее распространенный способ;)
          to think( думать; считать; полагать; представлять себе;)
          middle man (посредник;)
	-->


## Methods and actions ##

A Controller is a Ruby class which inherits from ActionController::Base and has methods just like any other class. Usually these methods correspond to actions in MVC, but they can just as well be helpful methods which can be called by actions. When your application receives a request, the routing will determine which controller and action to run. Then an instance of that controller will be created and the method corresponding to the action (the method with the same name as the action) gets run.

	class ClientsController < ActionController::Base
	  # Actions are public methods
	  def new
	  end
	
	  # These methods are responsible for producing output
	  def edit
	  end

	# Helper methods are private and can not be used as actions
	private

	  def foo
	  end
	end

Private methods in a controller are alse used as filters, which will be covered later in this guide.

As an example, if the user goes to '/clients/new' in your application to add a new client, a ClientsController instance will be created and the 'new' method will be run. Note that the empty method from the example above could work just fine because Rails will by default render the 'new.html.erb' view unless the action says otherwise. The 'new' method could make available to the view a '@client' instance variable by creating a new Client:

	def new
	  @client = Client.new
	end


	<!--
	  just fine (просто отлично)
	  render (выполнять)
	  otherwise (иным способом; другим способом;) 
	-->

