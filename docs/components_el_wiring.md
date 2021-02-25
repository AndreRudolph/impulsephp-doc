<h3 class="doc-title">Components Expression Language based Wiring</h3>

- [Introduction](#introduction)
- [Available functions](#available-functions)
    - [type()](#type)
	- [instanceOf()](#instanceOf)
    - [component(attribute)](#componentAttribute)
    - [id()](#id)
    - [uid()](#uid)
- [Options](#options)

<a id="introduction">Introduction</a>

Sometimes it is necessary to find direct descendant components by a condition within the same tree of components. This is the use case the annotation  **_Wire_** was designed for.

<a id="available-functions">Available functions</a>

You can define the component(s) that will be wired into an attribute of the class by using symfonys awesome expression language. Impulse extends the expression language by adding four different functions to retrieve components based on their state and class instance. The functions are explained in the sections below.

<h5><a id="type">type</a></h5>

The RadioGroup as an example component uses the expression language to wire the direct descendant radio components by using the type() expression language function. 

<div class="code-header">
	<div class="container-fluid">
		<div class="row">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
    </div>
</div>
<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php
	namespace Impulse\ImpulseBundle\UI\Components;
    use ...

    class RadioGroup extends AbstractComponent implements AfterCreateChilds
    {
        #[Wire('type("Radio")'))]
        private ?Collection $radios;
	}</code>
</pre>

This expression function matches all components with the class short name Radio either in the component class itself or interfaces or parent classes.  

<h5><a id="instanceOf">instanceOf()</a></h5>

The RadioGroup component uses the expression language to wire the direct descendant radio components by using the instanceOf() expression language function provied by Impulse. 

<div class="code-header">
	<div class="container-fluid">
		<div class="row">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
    </div>
</div>
<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php
	namespace Impulse\ImpulseBundle\UI\Components;
    use ...

    class RadioGroup extends AbstractComponent implements AfterCreateChilds
    {
        #[Wire('instanceOf("Impulse\\\\ImpulseBundle\\\\UI\\\\Components\\\\Radio")')]
        private ?Collection $radios;
	}</code>
</pre>

This function works the same like PHPs instanceof and also works with inheritance and interfaces.

<a id="componentAttribute">component(Attribute)</a>

A more generic function is the component function which has one parameter that retrieves an attribute value of the component for comparison. The following example will look for all components with the id **_specialId_** .

<div class="code-header">
	<div class="container-fluid">
		<div class="row">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
    </div>
</div>
<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php
    
    class SpecialComponent {

	    #[Wire('component("id") === "specialId"')]
        private ?Collection $radios;
    }</code>
</pre>

<a id="id">id()</a>

The example above evaluates and finds components with the id **_specialId_** . However, the Impulse framework provides a shortcut for this syntax with the id function. The following example is equivalent to the above example.

<div class="code-header">
	<div class="container-fluid">
		<div class="row">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
    </div>
</div>
<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php

    class SpecialComponent {

        #[Wire('id() === "specialId"')]
        private ?Collection $radios;
    }</code>
</pre>

<a id="uid">uid()</a>

Though this functions has no use cases yet, it can be useful in the future when you want to directly retrieve components with a certain uid on a concrete component instance.
