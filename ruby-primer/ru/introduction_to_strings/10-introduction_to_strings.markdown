﻿# 1.0 Введение в Strings #

## Построение String ##

В действиях лежит истинный путь, давайте начнем действовать. Наберите "Ruby Monk" в строке ниже ( не забудьте включить кавычки).

	"Ruby Monk"

Строки это ключ к взаимодействию с пользователем и новички в программировании столкнуться с ними и начнут управлять строками с ранней стадии путешествия. В этом уроке мы откроем несколько инструментов, которые предоставляет Ruby, чтобы создавать и управлять строками. 

## Буквенные(символьные) формы ##

Построение String есть то, что известно как символьная форма - интерпретатор пользует все что окружено одиночными кавычками ( ' ) или двойными кавычками ( " ) как строки. Другими словами, обе 'RubyMonk' и "RubyMonk" создадут экземляры строк.
Теперь ваша очередь; идите дальше и создайте строку, которая содержит имя текущего месяца. Как 'April', или 'December'.

	'June'

Действия оставляют более глубокий след в памяти чем простые слова. Заново создадим строку из упражнения выше используя двойные кавычки ( " ). Вы увидите, что тесты пройдут также как и в прошлый раз.

	"June"

Подходы одинарных и двойных кавычек имеют некоторые различия, в которые мы заглянем позже.
В большинстве случаев они равны. 

Все Strings являются экземплярами Ruby класса String, который предоставляет ряд методов для управления строкой. Теперь когда вы успешно освоили создание строк, давайте взглянем на наиболее часто используемые методы.

## Длина строки ##

Как программисту, одну из простых вещей которые необходимо знать о String это его длина. Вместо того, чтобы просто рассказать тебе как это сделать, позволь мне поделиться с тобой секретом о Ruby API: Попробуй очевидное!
Ядро библиотеки Ruby очень интуитивно и с небольшой практикой вы можете легко перевести из разговорного английского на Ruby.

Проверьте свою интуицию и попробуйте изменить код ниже, чтобы вернуть длину строки 'RubyMonk'.

	'RubyMonk' в 'RubyMonk'.length
	irb> 'RubyMonk'.length
	=> 8

Это не должно обременить твою интуицию слишком сильно! Давайте перейдем к немного более сложным понятиям.
                                                                                                         
