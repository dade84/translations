﻿# 0.2 Синтаксический сахар для спецальных методов #

## Специальные методы ##

Как внимательный читатель, вы возможно заметили в предыдущем уроке, целочисленные объекты вносят в список математические операторы такие как + и - в число их методов. Вы наверно также думали про себя, что вызов метода + также как 1.+(2) для сложения двух чисел будет...громоздким.

Да, хотя это работает просто отлично, попробуйте сами прибавать 4 к 3 в упражнении ниже.

	4.+(3)

Ruby как язык ставит своей целью быть чрезвычайно дружественным, таким образом можно смело предположить, что имеется наиболее лучший путь. Ruby делает исключения в своих синтаксических правилах для часто используемых операторов, таким образом, нет необходимости в использовании . для их вызова над объектами.
Давайте перефразируем предыдущий пример в более естественный синтаксис опустив точки и скобки.

	1+2 # также как 1.+(2)	   

Существуют несколько других имен методов у которых есть этот специальный статус - вот краткий обзор тех, с которыми веротяно столкнетесь.

	+   -   *   /   =   ==    !=    >   <   >=    <=    []

Последний метод ( [] ) возможно уже видели в уроке, который охватывает Arrays и является возможно самым уникальным в своем синтаксисе. Не только не требует точки, но также заключает аргументы в себя. Вот короткий пример, чтобы освежить вашу память:

	words = ["foo", "bar", "baz"]
	words[1]

Еще более интересным является то, что это все еще работает если вы используете более традиционный синтаксис метода - убедитесь сами, запустив пример ниже.

	words = ["foo", "bar", "baz"]
	words.[](1)

Это общий шаблон в Ruby - два различных способа сделать одну и ту же вещь где один остается совместимым, а другой изменяет синтаксис, чтобы быть наиболее дружелюбным.

