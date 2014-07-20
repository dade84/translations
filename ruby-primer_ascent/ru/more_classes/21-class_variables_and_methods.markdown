# 2.1 Class Variables and Methods #

## То, что мы уже знаем ##

Уже известно, что instance method это :

	class Item
	  def initialize(item_name)
	    @item_name = item_name
	  end

 	  def show
	    puts "Instance method show invoked for '#{self}'"
	  end

	  def to_s
	    "Item: #{@item_name}"
	  end
	end
	
	Item.new("selence").show

Оба метода initialize и show являются instance методами. Как и instance переменные, они работают с экземплярами класса.

Давайте сравним их с методами класса:

	class Item
	  def self.show
	    puts "Class method show invoked"
	  end
	end

	Item.show

В def self.show, ключевое слово self означает, что метод show будет определен в контексте самого класса, не его экземпляров. Любое определение метода без определения self является по умолчанию instance методом.

Существует также другой способ для того, чтобы определить методы класса, на которые вы можете натолкнуться в коде Ruby:

	class Item

	  class << self
	    def show
	      puts "Class method show invoked"
	    end
	  end

	end

Методы класса(class methods) не имеют доступа к instance methods или instance variables. Тем не менее instance methods могут иметь доступ к class methods и class variables.

В следующем разделе, увидим как можно использовать методы класса и переменные класса для того чтобы совершить что-то полезное.

## Использование переменных класса для того, чтобы сохранить настройки приложения ##

В следующем примере, мы определим класс Planet, который содержит количество экземпляров класса Planet.

Напомним то, что переменная экземпляра всегда имеет символ @ перед своим именем. Подобно этому, переменные класса имеют @@ перед своими именами.

	class Planet
	  @@plantes_count = 0

	  def initialize(name)
	    @name = name
	    @@planets_count += 1
	  end

	  def self.planets_count
	    @@planets_count
	  end
	end

	Planet.new("earth"); Planet.new("uranus")

	p Planet.planets_count

Видно, что переменные класса используются для того, чтобы сохранить данные, котоыре принадлежать классу, но не его экземплярам.

Имеется много случаев, когда нужно использовать переменные класса. В действительности, его неподходящее использование вызывает неодобрение в основном в сообществе Ruby.

Одно из мест, где переменные класса находят правльное использование это соханение настроек приложения - такие вещи как, имя приложения, версия, база данных и другие настройки. В следующем упражнении необходимо создать один из таких классов ApplicationConfiguration. Ты должен определить два метода уровня класса: set и ge. Метод set принимает два параметра: property_name и value. Метод get принимает один параметр: property_name и необходим вернуть значение, которое соответствует этому свойству.

	class ApplicationConfiguration
	  @@properties = {}

	  def self.set(property_name, value)
	    @@properties[property_name] = value
	  end
  
	  def self.get(property_name)
	    @@properties[property_name]
	  end  
	end

	ApplicationConfiguration.set("name", "Demo Application")
	ApplicationConfiguration.set("version", "0.1")

	p ApplicationConfiguration.get("version")

## Переменные класса и наследование ##

В упражнение выше, вы создали простой класс ApplicationConfiguration, который будет служить нуждам одного приложения достаточно хорошо. Но что случится, когда у меня несколько приложений и необходимо сохранить их настройки таким образом?

Самый простой ответ это создать новый класс, один для каждого приложения и дать им отнаследоваться от ApplicationConfiguration таким образом мы получим set и get методы. Посмотрите как это работает:

	class ApplicationConfiguration
	  @@configuration = {}

	  def self.set(property, value)
	    @@configuration[property] = value
	  end

	  def self.get(property)
	    @@configuration[property]
	  end
	end

	class ERPApplicationConfiguration < ApplicationConfiguration
	end

	class WebApplicationConfiguration < ApplicationConfiguration
	end

	ERPApplicationConfiguration.set("name", "ERP Application")
	WebApplicationConfiguration.set("name", "Web Application")

	p ERPApplicationConfiguration.get("name")
	p WebApplicationConfiguration.get("name")

	p ApplicationConfiguration.get("name")

Я уверен, что вы выясните, что здесь происходит. Несмотря на то, что у меня два класса, изменения в одном приводит к изменениям в другом. Это также означает, что базовый класс ApplicationConfiguration тоже подвержен измененям, когда что-либо меняется в наследуемом классе.

Тот факт, что любое изменение в любом из таких классов ApplicationConfiguration, ERPApplicationConfiguration или WebApplicationConfiguration будут влиять на остальне два. Они совместно используют одну и туже копию переменной класса @@configuration. Это то, как работает наследование класса. Но это не то поведение которое мы хотим!

Ответ на это лежит в другом типе переменных класса: class instance variables.

Вот пример определения class instance variable и как это работает с наследованием:

	class Foo
	  @foo_cont = 0

	  def self.increment_counter
	    @foo_count += 1
	  end

	  def self.current_count
	    @foo_count
	  end
	end

	class Bar < Foo
	  @foo_count = 100
	end

	Foo.increment_counter
	Bar.increment_counter
	p Foo.current_count
	p Bar.current_count


Class instance variable в примере выше это @foo. Несмотря на то, что форма записи сходна с instance переменной, различие здесь в том, что @foo инициализируется напрямую в теле класса и доступ осуществляется только в методах класса.

Видно, что значения @foo_count отличаются для Foo и Bar. Это означает, что оба класса влиюят на различные @foo_count. Обратите внимание, что необходимо проинициализировать @foo_count во всех классах наследниках.

Сейчас, можете ли вы поправить пример ApplcationConfiguration для того, чтобы он работал правильно для обоих класса родителя и класса наследника?

	class ApplicationConfiguration
	  @configuration = {}

	  def self.set(property, value)
	    @configuration[property] = value
	  end

	  def self.get(property)
	    @configuration[property]
	  end
	end

	class ERPApplicationConfiguration < ApplicationConfiguration
	  @configuration = {}
	end

	class WebApplicationConfiguration < ApplicationConfiguration
	  @configuration = {}
	end

	ERPApplicationConfiguration.set("name", "ERP Application")
	WebApplicationConfiguration.set("name", "Web Application")

	p ERPApplicationConfiguration.get("name")
	p WebApplicationConfiguration.get("name")

	p ApplicationConfiguration.get("name")

Class instance variables лучшая альтернатива class variables просто потому что данные не используются совместно через всю цепочку наследования. Другое главно отличие между class variables и class instance variables в том, что class instance variables доступны только в методах класса. Но переменные класса доступны в class methods и instance methods.

Краткий обзор прежде чем пойдем дальше:

* Instance variables доступны только экземплярам класса. Они выглядят как @name. Class variables доступны class methods и instance methods. Они выглядят как @@name.
* Почти всегда плохая идея использовать переменную класса для сохранения состояния. Имеется очень мало вариантов использования где переменные класса являются правильным выбором.
* Предпочтительны class instance variables над class variables когда вам действительно необходимо сохранить данные на уровне класса. Class instance variables используют ту же самую форму записи, что и instance variables. Но в отличие от instance variables, вы объявляете их прямо внутри определения класса.



