﻿# 3.2 Повторение(Итерация) #

## Доберемся до циклов 'for'  ##

Ruby как и большинство языков имеет излюбленный цикл for. Интересно, но никто не использует его много - но давайте оставим альтернативы для следующего упражнения. Вот как используется цикл for в Ruby. Просто запустите код ниже чтобы распечатать все значения, содержащиеся в массиве.

	array = [1, 2, 3, 4, 5]
	for i in array
	  puts i
	end

Ок, сейчас ваша очередь. Скопируйте значения меньше 4 в массив, сохраненный в переменной source в массив, который содержится в переменной destination.

	def array_copy(source)
	  destination = []
	  for i in source
    	    destination << i if i < 4
  	  end  
	  return destination
	end

## цикличность с 'each' ##

Повторение это один из самых распространенных цитируемых примеров использования блоков в Ruby. Метод Array#each принимает блок, к которому каждый элемент массива передается по очереди. Вы увидите, что циклы for почти никогда не испольузются в Ruby и Array#each и его братья и сестры являются стандартом де-факто. Мы тщательно рассмотрим относительные достоинства использования each над циклами for немного позже как только будет время, чтобы познакомиться с ними. Давайте посмотрим на пример, который распечатывает все значения массива.

	array = [1, 2, 3, 4, 5]
	array.each do |i|
	  puts i
	end

Ок, давайте сейчас попробуем тоже самое, что сделали циклом for ранее - скопировали значения, которые меньше 4 из массива, сохраненного в переменной source в массив переменной destination.

	def array_copy(source)
	  destination = []
	  source.each {|i| destination << i if i < 4}
	  return destination
	end 


