# 2.4 Сереализация #

## Сереализация? Сириализация? Что? ##

В какой-то момент, вы захотите, чтобы ваша Ruby программа могла общаться с другими программами. Обычно если ваша программа общается с другой программой, общение между ними осуществлялось через API(application programmin interface). Не позволяйте статье напугать вас. API это просто основная формулировка для любого интерфейса, который позволяет одной программе либо считывать данные другой программы либо писать данные в другую программую. Или и читать и писать!

Один из самых простых интерфейсов, которые могут разделять две Ruby программы это файл. Первая программа будет трансформировать некоторые объекты в строку и затем писать эти строки в файл. Вторая программа будет искать файл в общем месте(это особенно легко если обе программы запущены на одном компьютере), считывать и преобразовать строку обратно в Ruby объект. Посмотрите на главу I/O книги Ruby Primer, если вам необходим обновить в памяти библиотеки для работы с файлами Ruby.

Основная идея (перемещение данных между программами), в основном это то на что мы ссылаемся когда говорим "сереализация". Преобразовывая наши Ruby объекты в строки мы сделали их символьными: все объекты объединены вместе в строку символов, один символ после другого. На самом деле мы рассмотрели один метод сереализации: Object#to_s. В упражнении ниже, попробуйте написать метод to_s, который производит строку (from_s understands).

	class CerealBox
	  attr_accessor :ounces, :contains_toy
  
	  def initialize(ounces, contains_toy)
	    @ounces = ounces
	    @contains_toy = contains_toy
	  end
  
	  def self.from_s(s)
	    ounces = 0
	    contains_toy = false
	    s.each_line do |field|
	      value = field.split(":")[1].strip
	      ounces = value.to_i if field.include?("ounces")
	      contains_toy = to_boolean(value) if field.include?("contains_toy")
	    end
	    CerealBox.new(ounces, contains_toy)
	  end
  
	  def self.to_boolean(str)
	    str == 'true'
	  end
  
	  def to_s
	    "ounces: #{@ounces}\n contains_toy: #{@contains_toy}"
	  end
	end  

Пока эта стратегия работает, она очень громоздка. Метод from_s является большим, неприятным и хрупким. Попробуй передать ему неправильное значение! Он не будет очень рад этому. It's also not very generic$ мы должны были бы написать пользовательский метод from_s для каждого нового класса. Ух! Давайте даже не будем думать об этом. Вместо это давайте переместимся во встроенное решение Ruby для этой проблемы.


## Выгрузка и загрузка ##

Нам нужен некоторый способ, чтобы автоматически трансформировать Ruby объект в символьную(сериализованную) форму. Существует целый ряд форматов сериализации, которые мы можем использовать. Вы должно быть слышали о XML, json или YAML. Ruby может производить любые из этих форматов(все они строковые кстати), но мы, в частности, взглянем на YAML посколько он встроен в язык Ruby. Вот, YAML::dump и YAML::load делюат все трудные перемещения за нас.

	class Ogre
	  def initialize(strength, speed)
	    @strength = strength
	    @speed = speed
	  end

	  def self.deserialize(yaml_string)
	    YAML::load(yaml_string)
	  end

	  def serialize
	    YAML::dump(self)
	  end

	  def to_s
	    "Ogre: [strength = #{@strength}, speed = #{@speed}]"
	  end
	end

	wendigo = Ogre.new(47,3)
	yaml = wendigo.serialize
	puts "The yaml looks like this:"
	p yaml
	puts "It's just a boring old string:#{yaml.class}"
	puts "...and it's easy to change back: #{Ogre.deserialize(yaml)}"  
	puts

	shrek = Ogre.new(62,12)
	fiona = Ogre.new(66,37)
	ogres = [shrek, fiona]
	puts "We can even serialize arrays! They're just another object. #{ogres}"

Отлично, правильно? Намного легче чем ручное решение

## Нажмите "start" для того, чтобы сохранить игру ##

Как вы уже наверное заметили, идея передачи данных между программами не ограничена двумя различными программами. В примерах выше было чтение и запись их собственных форматов сериализации. Теоретически, если мы запишем некоторые Ruby объекты в файл, закроем программу, затем откроем нашу программу позже и загрузим эти самые объекты, мы на самом деле переместим данные между двумя программами: время нашей программы Т и время нашей программы T+n в будущем! На самом деле, тут ничего особенного. Это то, как ваш текстовый процессор открывает в четверг файл, созданный в понедельник. Но это звучит покруче этого способа.

Давайте использовать этот подход для того, чтобы расширить наш пример Ogre. Как можно видеть из класса Ogre, YAML понимает любой Ruby объект, который вы бросаете(подкидываете) ему. Это означает, что нам не нужно класть вызов YAML метода в сам класс Ogre - взаимодействие с YAML может происходить на более высоком уровне, как наш метод save_game ниже. Чтобы завершить упражнение, чтобы персонажи игры могли быть сохранены на диск и загружены позже. Чтобы сохранить YAML строку на диск, используй удобные методы GameFile#write(yaml) и GameFile#read, которые я написал для вас. GameFile.new принимает один параметр, которому следует быть .yaml файлом.

	class Ogre
	  attr_accessor :strength, :speed, :smell
	  def initialize(strength, speed, smell)
	    @strength = strength
	    @speed = speed
	    @smell = smell
	  end
	end

	class Dragon
	  attr_accessor :strength, :speed, :color
	  def initialize(strength, speed, color)
	    @strength = strength
	    @speed = speed
	    @color = color
	  end
	end

	class Fairy
	  attr_accessor :strength, :speed, :intelligence
	  def initialize(intelligence)
	    @strength = 1
	    @speed = 42
	    @intelligence = intelligence
	  end
	end

	def save_game(characters)
	  yaml = YAML::dump(characters)
	  game_file = GameFile.new("/my_game/saved.yaml")
	  game_file.write(yaml)
	end

	def load_game
	  game_file = GameFile.new("/my_game/saved.yaml")
	  yaml = game_file.read
	  YAML::load(yaml)
	end

