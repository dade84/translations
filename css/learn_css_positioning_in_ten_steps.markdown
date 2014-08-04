http://www.barelyfitz.com/screencast/html-training/css/positioning/
# Learn CSS Positioning in Ten Steps #

	<!--
	  positioning (�����������, ����������������; ����������; ��������(��� �������))
	-->

This tutorial examines the different layout properties available in CSS: position:static, position:relative, position:absolute, and float.

	<!--
	  to examine (�������������; �����������; �����������; �������)
	  layout (������������;)

	-->

1. position:static

The default positioning for all elements is position:static, which means the element is not positioned and occurs where it normally would in the document.
Normally you wouldn't specify this unless you need to override a positioning that had been previously set.

	#div-1 {
	  position:static;
	}

	<!--
	normally (��� �������, �� ����; ������;)
	-->

2. position:relative

If you specify position:relative, then you can use top or bottom, and left or right to move the element relative to where it would normally occur in the document.

Let's move div-1 down 20 pixels, and to the left 40 pixels:

	#div-1 {
	  poisition: relative;
	  top: 20px;
	  left: -40px;
	}

Notice the space where div-1 normally would have been if we had not moved it: now it is empty space. The next element(div-after) did not move when we moved div-1. That's because div-1 still occupies that original space in the document, even though we have moved it.
It appears that position:relative is not very useful, but it will perform an important task later in this tutorial.
	<!--
	  occur (�����������;)
	  occupy (��������;)
	  it appiears that (���� �� �����; ����������, ���; ��-��������; �����������;)
	  perform (���������;���������;������;)
	-->

3. position:absolute

When you specify position:absolute, the element is removed from the document and placed exactly where you tell it to go.
Let's move div-1a to the top right of the page:
	#div-1a {
	  position: absolute;
	  top: 0;
	  right: 0;
	  width: 200px;
	}

Notice that this time, since div-1a was removed from the document, the other elements on the page were positioned differently: div-1b, div-1c, and div-after moved up since div-1a was no longer there.
Also notice that div-1a was positioned in the top right corner of the page. It's nice to be able to position things directly the page, but it's of limited value.
What I really want is to position div-1a relative to div-1. And that's where relative position comes back into play.

## Footnotes ##
* There is a bug in the Windows IE browser: if you specify a relative width(like "width: 50%") then the width will be based on the parent element instead of on the positioning element.

 	<!--
	to position (�������; ���������; ���������� ��������������; ����������;)
	footnotes (������; ����������;)
	-->




