<!-- html5doctor.com/avoiding-common-html5-mistakes/ -->
# Avoiding common HTML5 mistakes #
<!-- to avoid (избегать) -->

Between curating sites for the HTML5 gallery and answering readers' questions here at HTML5 Doctor, I see a host of HTML5 sites and their underlying markup. In this post, I'll show you some of the mistakes and poor markup practices I often see and explain how to avoid them.

<!-- to curate (курировать-вести, направлять, опекать) 
underlying (лежащий в основе, основной)
post (сообщение на форуме)
to post (опубликовать в Web)
poor practise (неправильные методы;низкий уровень)

-->

## Don't use section as a wrapper for styling ##

<!-- styling (дизайн) -->

One of the most common problems I see in people's markup is the arbitrary replacement of <div>s with HTML5 sectioning elements - specifically, replacing wrapper <div>s (used for styling) with <section>s. In XHTML or HTML4, I would see something like this:

<!-- common (общий; распространенный; обычный; частый;)
arbitrary (произвольный; самовольный; необоснованный; субъективный; без уважительных причин)
specifically (особенно; а именно; в частности; главным образом;)
-->

	<!-- HTML 4-style code -->
	<div id="wrapper">
	  <div id="header">
	    <h1>My super duper page</h1>
	    <!-- Header content -->
	  </div>
	  <div id="main">
	    <!-- Page content -->
	  </div>
	  <div id="secondary">
	    <!-- Secondary content -->
	  </div>
	  <div id="footer">
	    <!-- Footer content -->
	  </div>
	</div>

Now, I'm instead seeing this:

	<!-- Don't copy this code! It's wrong! -->
	<section id="wrapper">
	  <header>
	    <h1>My super duper page </h1>
	    <!-- Header content -->
	  </header>
	  <section id="main">
	    <!-- Page content -->
	  </section>
	  <footer>
	    <!-- Footer content -->
	  </footer>
	</section>

	<!-- duper (пупер) -->

Frankly, that's just wrong: <section> is not a wrapper. The <section> element denotes a semantic section of your content to help construct a document outline. It should contain a heading. If you're looking for a page wrapper element (for any flavour of HTML of XHTML), consider applying styles directly to the <body> element as described by Kroc Camen. If you still need and addition element for styling, use a <div>. As Dr Mike explains, div isn't dead, and if there's nothing else more appropriate, it's probably where you really want to apply your CSS.

<!-- Frankly (прямо; открыть; откровенно; откровенно говоря; прямо скажем; если честно;)
to denote (обозначать; означать;)
outline (иерархическая структура документа(вчт.))
flavour (вид)
consider (рассмотреть вопрос)
appropriate ( соответствующий; подходящий;)

-->


With that in mind, here's the correct way to mark up the above example using HTML5 and a couple of ARIA roles. (Note: you may need on <div> depending on your design.)

<!-- ARIA roles Accessible Rich Internet Applications http://www.w3.org/TR/wai-aria/ -->

	<body>
	  <header>
	    <h1>My syper duper page</h1>
	    <!--Header content -->
	  </header>
	  <div role="main">
	    <!-- Page content -->
	  </div>
	  <aside role="complementary">
	    <!-- Secondary content -->
	  </aside>
	  <footer>
	    <!-- Footer content -->
	  </footer>
	</body>

If you're not quite sure which element to use, then I suggest you refer to our HTML5 sectioning content element flowchart to guide you along your way.

<!-- to guide (вести; направлять;) -->

## Only use header and hgroup when they're required ##

	the <hgroup> has now been removed from the HTML5 specification, please keep that in mind when reading the rest of this article.

<!-- the rest of (остальной; оставшийся; остальная часть чего-либо;) -->


There's no sense writing markup when you don't have to, right? Unfortunately, I often see <header> and <hgroup> being used when there's no need for them. You can get up to speed with our articles on the <header> element and the <hgroup> element, but I'll briefly summarise:

* The <header> element represents a group of introductory or navigational aids and usually contains the section's heading
* The <hgroup> element groups a set of <h1>-<h6> elements representing a section's heading when the heading has multiple levels, such as subheadings, alternative titles, or taglines

<!-- sense (смысл; чуство;)
unfortunately (к сожалению; к несчастью;)
get up to speed (вывести на требуемый уровень; ввести в курс событий,дела)
heading (заголовок;)
tagline (слоган;)

--> 


## Overuse of header ##
<!-- overuse (злоупотреблять; чрезмерно использовать;) -->

As I'm sure you're aware, the <header> element can be used multiple times in a document, which has popularized the following pattern:

	<!-- Don't copy this code! No need for header here -->
	<article>
	  <header>
	    <h1>My best blog post</h1>
	  </header>
	  <!-- Article content -->
	</article>

<!-- 
aware (сознающий; знающий; осведомленный;)
popularize (распространять; пропагандировать; излагать популярно; популяризировать;)

 -->

If your <header> element only contains a single heading element, leave out the <header>. The <article> ensures that the heading will be shown in the document outline, and because the <header> doesn't contain multiple elements (as the definition describes), why write code when you don't need to? Simply use this:
	
	<article>
	  <h1>My best blog post</h1>
	  <!-- Article content -->
	</article>

<!-- 
leave out (не влючать; исключать; пропустить; )
to ensure (гарантировать; обеспечивать; ручаться;)

-->


## Incorrect use of <hgroup> ##

On the subject of headers, I alse frequently see incorrect uses of <hgroup>. You should not use <hgroup> in conjunction with <header> when:

* there's only one child heading, or
* the <hgroup> would work fine on its own (i.e., without the <header>).

<!-- 
on the subject of (по поводу)
on its own (сам/сама по себе; собственными силами)

-->

The first problem looks like this:

	<!-- Don't copy this code! No need for hgroup here -->
	<header>
	  <hgroup>
	    <h1>My best blog post</h1>
	  </hgroup>
	  <p>by Rich Clark</p>
	</header>

In that case, simply remove the <hgroup> and let the heading run free.

	<header>
	  <h1>My best blog post</h1>
	  <p>by Rich Clark</p>
	</header>

<!-- 
let run free (выпустить; освободить;)

-->

The second problem is another case of including elements when they're not necessarily required.

	<!-- Don't copy this code! No need for header here -->
	<header>
	  <hgroup>
	    <h1>My company</h1>
	    <h2>Established 1893</h2>
	  </hgroup>
	</header>

When the only child of the <header> is the <hgroup>, why include the <header>? If you don't have additional elements within the <header> (i.e., siblings of the <hgroup>), simply remove the <header> altogether.
	
	<hgroup>
	  <h1>My company</h1>
	  <h2>Established 1893</h2>
	</hgroup>

For more <hgroup> examples and explanations, read the detailed article about it in the archives.

## Don't wrap all lists of links in <nav> ##

With 30 new elements (at the time of writing) introduced in HTML5, we're somewhat spoilt for choice when it comes to creating meaningful, structured markup. That said, we shouldn't abuse the super-semantic elements now made available to us. Unfortunately, that's what I see happening with <nav>. The spec describes <nav> as follows:

	"The nav element represents a section of a page that links to other pages or to parts within the page: a section with navigation links.
	
	Note: Not all groups of links on a page need to be in a nav element - the element is primarily intended for sections that consist of major navigation blocks. In particular, it is common for footers to have a short list of links to various pages of a site, such as the terms of service, the home page, and a copyright page. The footer element alone is sufficient for such cases; while a nav element can be used in such cases, it is usually unnecessary.
	WHATWG HTML spec

<!--
somewhat (немного; до некоторой степени; несколько; в какой-то степени;)
spoilt for choice (избалованный выбором);
when it comes (когда дело доходит;)
meaningful (осмысленный; значащий;)
abuse (чрезмерно увлекаться;)
intend (преполагать;предназначать;)
terms of service (условия оказания услуг;)
alone (как таковой; взятый отдельно; взятый сам по себе;)
sufficient (довольно; достаточно; )
WHATWG 	Web Hypertext Application Technology Working Group
-->


The key phrase is 'major' navigation. We could debate all day about the definition of 'major', but to me it means:

* Primary navigation
* Site search
* Secondary navigation(arguably)
* In-page navigation (within a long article, for example)

<!-- 
arguably (возможно)
-->

While there isn't any right or wrong here, a straw poll coupled with my own interpretation tells me that the following shouldn't be enclosed by <nav>:

* Pagination controls
* Social links (although there may be exceptions where social links are the major navigation, in sites like About me or Flavours, for example)
* Tags on a blog post
* Categories on a blog post
* Tertiary navigation
* Fat footers

<!-- 
straw poll (предварительное голосование; неофициальный опрос)
coupled with (в сочетании с; вкупе с; наряду с;)

-->

If you're unsure whether to mark up a list of links with <nav>, ask yourself this: "Is this major navigation?" To help answer the question, consider the following rules of thumb:

* "Don't use <nav> unless you think <section> would also be appropriate, with an <hx>" - Hixie on IRC
* Would you add a link to it in a 'skip to' block for accessibility?

If the answer to these questions is 'no', then it's probably not a <nav>.

<!-- 
unsure (неуверенный; не гарантированный; небезопасный; колеблющийся;)
consider (рассматривать; считать;)
rules of thumb (практические методы; правила на основе практики;)
appropriate (соответствующий; оптимальный; подходящий;)
-->

## Common mistakes with the figure element ##

Ah, <figure>. The appropriate use of this element, along with its partner-in-crime <figcaption>, is quite difficult to master. Let's look at a few common problems I see with <figure>.

<!--
partner-in-crime (сообщник;подельник; соучастник преступления;)
along with (вместе; наряду с; вместе с;)
-->

## Not every image is a figure ##

Earlier, I told you not to write extra code when it's not nesessary. This mistake is similar. I've seen sites where every image has been marked up as a <figure>. There's no need to add extra markup around your images for the sake of it. You're just creating more work for yourself and not describing your content any more clearly.

The spec describes <figure> as being "some flow content, optionally with a caption, that is self-contained and is typically referenced as a single unit from the main flow of the document." Therein lies the beauty of <figure>, that it can be moved away from the primary content - to a sidebar, say - without affecting the document's flow.

If it's a purely presentational image and not referenced elsewhere in the document, then it's definitely not a <figure>. Other use cases vary, but as a start, ask yourself, "Is this image required to understand the current context?" If not, it's probably not a <figure> (an <aside>, perhaps). If so, ask yourself, "Could I move this to an appendix?" If the answer to both questions is 'yes', it's probably a <figure>.

<!--
extra (дополнительный)
mark up (размечать; повысить цену; вести счет;)
markup (разметка;)
for the sake of it (просто так;)
optionally (необязательно; при желании; в качестве опции; добавочный; дополнительный;)
caption (сопроводительная подпись; заголовок главы; заглавие; подпись к картинке;)
self-contained (самостоятельный; независимый; замкнутый; автономный;)
typically (типично; как правило; часто; в основном; обычно;)
single unit (одну единица)
therein (в этом; в этом отношении;)
therein lies (в этом заключается;)
moved away (отодвинутый;)
sidebar (боковая панель;)
affect (затрагивать;)
purely (чисто; совершенно; полностью; только; лишь; просто;)
vary (менять; меняться; изменяться; отличаться;расходиться; варьировать;)
appendix (приложение;)
-->

## Your logo is not a figure ##

Segueing nicely on, the same applies to your logo. Here are a couple of snippets that I see requlary:

	<!-- Don't copy this code! It's wrong! -->
	<header>
	  <h1>
	    <figure>
	      <img src="/img/mylogo.png" alt="My company" class="hide" />
	    </figure>
	    My company name
	  </h1>
	</header>

	<!-- Don't copy this code! It's wrong! -->
	<header>
	  <figure>
	    <img src="/img/mylogo.png" alt="My company" />
	  </figure>
	</header>

There’s nothing else to say here. It’s just plain wrong. We could argue until the cows come home about whether your logo should be in an <h1>, but that’s not what we’re here for. The real issue is the abuse of the <figure> element. <figure> should be used only when referenced in a document or surrounding sectioning element. I think it’s fair to say that your logo is rarely going to be referenced in such a way. Quite simply, don’t do it. All you need is this:

	<header>  
	  <h1>My company name</h1>
	  <!-- More stuff in here -->
	</header>

<!--
seguing (продолжая)
snippet (фрагмент кода; фрагмент; короткая текстовая информация по сайту;)
plain (явный; простой;ясный; плоский;однотонный;)
argue (спорить;приводить доводы; доказывать; рассуждать; высказываться;)
until the cows come home (до бесконечности;)
abuse (неправильное употребление;)
fair (честный; справедливый;законный;)
rarely (редко; изредка; иногда;)

-->


## Figure can be more than an image ##

Another common misconception regarding <figure> is that it can only be used for images. This isn’t the case. A <figure> could be video, audio, a chart (in SVG, for example), a quote, a table, a block of code, a piece of prose, or any combination of these and much more besides. Don’t limit your <figure>s to images. Our job as web standards affectionadoes is to accurately describe the content with our markup.

A while back, I wrote about <figure> in more depth. It’s worth a read if you want more detail or need a refresher.
http://html5doctor.com/the-figure-figcaption-elements/

<!--
misconception (неправильное понимание; недоразумение; ложное представление;)
regarding (по вопросу; касаемо; касательно;)
this is not the case (это не тот случай; дело обстоит не так;)
chart (диаграмма)
quote (цитата)
piece of prose (отрывок прозы)
besides ()
affectionadoes ()
a while back (некоторое время тому назад)
it is worth (имеет смысл)
refresher (повторный)

-->

## Don’t include unnecessary type attributes ##

This is probably the most common problem we see in HTML5 Gallery submissions. While it isn’t really a mistake, I believe it’s best practice to avoid this pattern.

In HTML5, there’s no need to include the type attribute on <script> and <style> elements. While these can be a pain to remove if they’re automatically added by your CMS, there’s really no reason to include them if you’re coding by hand or if you have tight control over your templates. Since all browsers expect scripts to be JavaScript and styles to be CSS, you don’t need this:

	<!-- Don’t copy this code! It’s attribute overload! -->
	<link type="text/css" rel="stylesheet" href="css/styles.css" />
	<script type="text/javascript" src="js/scripts.js"></script>


Instead, you can simply write:

	<link rel="stylesheet" href="css/styles.css" />
	<script src="js/scripts.js"></script>

You can also reduce the amount of code you write to specify your character set, amongst other things. Mark Pilgrim’s chapter on semantics in Dive into HTML5 explains all.

<!--
unnecessary (ненужный; лишний;)
submission (представление;)
reduce (снизить; уменьшить; снижать;)
amongst other things (помимо прочего;)
-->

## Incorrect use of form attributes ##

You may be aware that HTML5 has introduced a range of new attributes for forms. We’ll cover them in more detail in the coming months, but in the meantime, I’ll quickly show you a few things not to do.

<!--
range (ряд)
in the meantime (в то же время; между тем; а пока; тем временем;)
-->

## Boolean attributes ##

Several of the new form attributes are boolean, meaning their mere presence in the markup ensures the behaviour is set. These attributes include:

* autofocus
* autocomplete
* required

Admittedly, I only rarely see this, but using required as an example, I’ve seen the following:

	<!-- Don’t copy this code! It’s wrong! -->
	<input type="email" name="email" required="true" />
	<!-- Another bad example -->
	<input type="email" name="email" required="1" />

Ultimately, this causes no harm. The client’s HTML parser sees the required attribute in the markup, so its function will still be applied. But what if you flip the code on its head and type required="false"?

	<!-- Don’t copy this code! It’s wrong! -->
	<input type="email" name="email" required="false" />

The parser will still see the required attribute and implement the behaviour even though you tried to tell it not to. Certainly not what you wanted.

There are three valid ways for a boolean attribute to be applied. (The second and third options only apply if you’re writing XHTML syntax.)

* required
* required=""
* required="required"

Applying that to our above example, we would write this (in HTML):

	<input type="email" name="email" required />

<!--
mere (явный; простой;)
presence (присутствие;)
Admittedly, (нужно признать, что;)
only (крайне)
ultimately (в конце концов; в конечном итоге; в конечном счете;)
to cause (вызывать;)
harm (вред;)
flip (перевернуть;)

-->
	
## Summary ##

It would be impossible for me to list here all the quirky markup patterns and practices I’ve come across, but these are some of the most frequently seen. What other common markup mistakes have you spotted around the web? Which areas of HTML5 require further clarification? We’d love to help get the wording or examples in the spec changed to make them a little more specific, so leave your thoughts below, and don’t forget to escape your HTML!

Thanks to Ian Devlin, Derek Johnson, Tady Walsh, the HTML5 Gallery curators, and the HTML5 Doctors for their input to this article.

<!--
quirky (изворотливый)
come across (натолкнуться; повстречаться; случайно встретиться;)
to spot (обнаружить;заметить;)
clarification (прояснение; выяснение; разъяснение;)
to escape (спасаться;)
-->



