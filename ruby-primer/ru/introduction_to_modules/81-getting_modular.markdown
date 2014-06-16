# 8.1 Getting Modular #

## Перемешиваем ##

Модули Ruby позволяют тебе создавать группы методов, которые ты потом можешь включать include и примешивать mix into в любые классы. Модули только содержат поведение, в отличие от классов, которые содержать и поведение и состояние.

Поскольку модули не могут быть созданы(не могу быть экземпляры), невозможно вызвать методы напрямую. Вместо этого они должны быть включены в другой класс, который делает его методы доступными экземплярам этого класса. Этого конечно, много для текущей книги, давайте не будем усложнять.

Для того, чтобы включить модуль в класс мы испольуем метод include, который принимает один параметр имя Module-я.

	module WarmUp
	  def push_ups
	    "Phew, I need a break!"
	  end
	end

	class Gym
	  include WarmUp
		
	  def preacher_curls
	    "I'm building my biceps."
	  end
	end

	class Dojo
	  include WarmUp
	
	  def tai_kyo_kyu
	    "Look at my stance!"
	  end
	end

	puts Gym.new.push_ups
	puts Dojo.new.push_ups

В примере выше, имеются классы Gym и Dojo. У каждого есть свое собственное поведение - preacher_curls и tai_kyo-kyu соответственно. Тем не менее, оба предусматривают push_ups, таким образом это поведение выделено в модуль, который затем вклюен в оба класса.

## Иерархия и упражнение ##

Так же, как и все классы, которые являются экземплярами Ruby Class, все модули в Ruby являются экземплярами Module.

Интересно, Module это суперкласс Class, это означает, что все класыы также являются модулями и могут использоваться как таковые. Для детального изучения урока наследования в Ruby, взгляни на главу в книге "Ruby Primer: Ascent."

	module WarmUp
	end

	puts WarmUp.class
	puts Class.superclass
	puts Module.superclass

Время для практики! Как всегда, пройдем тесты. Обратите внимание, что периметр кадрата и прямоугольника является вычисление суммы всех его сторон.

	module Perimeter
	  # Your code here
	  def perimeter
	    return sides.inject(0){|sum, number| sum + number }
	  end
	end

	class Rectangle
	  include Perimeter
	  # Your code here
	  def initialize(length, breadth)
	    @length = length
	    @breadth = breadth
	  end

	  def sides
	    [@length, @breadth, @length, @breadth]
	  end
	end

	class Square
	  include Perimeter
	  # Your code here
	  def initialize(side)
	    @side = side
	  end

	  def sides
	    [@side, @side, @side, @side]
	  end
	end

