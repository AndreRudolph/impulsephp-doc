<h3 class="doc-title">Components Expression Language based Wiring</h3>

- [Introduction](#introduction)
- [Available functions](#available-functions)
    - [type()](#type)
	- [instanceOf()](#instanceOf)
    - [component(attribute)](#componentAttribute)
    - [id()](#id)
    - [uid()](#uid)
- [Options](#options)

<h4><a id="introduction">Introduction</a></h4>

Sometimes it is necessary to find direct descendant components by a condition within the same tree of components. This is the use case the annotation  **_Wire_** was designed for.

<h4><a id="available-functions">Available functions</a></h4>

You can define the component(s) that will be wired into an attribute of the class by using symfonys awesome expression language. Impulse extends the expression language by adding five different functions to retrieve components based on their state and class instance. The functions are explained in the sections below.

<h5><a id="type">type()</a></h5>

The probably most used and useful functions is the type function. It works almost similar as the <a href="#instanceOf">instanceOf()</a> function below with one exception. It compares only the short name of the class (like Radio in the example below) instead of its full qualified namespace.

<div class="code-header">
	<div class="container-fluid">
		<div class="row">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
    </div>
</div>
<pre class="code-white language-php">
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

Unlike the type function the instanceOf function consideres the full qualified namespace for matching.

<div class="code-header">
	<div class="container-fluid">
		<div class="row">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
    </div>
</div>
<pre class="code-white language-php">
	<code class="imp-code language-php"><?php
	namespace App\UI\Components;
    use ...

    class SpecialComponent extends AbstractComponent
    {
        #[Wire('instanceOf("Impulse\\\\ImpulseBundle\\\\UI\\\\Components\\\\Radio")')]
        private ?Collection $radios;
	}</code>
</pre>

This function works the same like PHPs instanceof and also works with inheritance and interfaces.

<h5><a id="componentAttribute">component(Attribute)</a></h5>

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
<pre class="code-white language-php">
	<code class="imp-code language-php"><?php
    
    class SpecialComponent {

	    #[Wire('component("id") === "specialId"')]
        private ?Collection $radios;
    }</code>
</pre>

<h5><a id="id">id()</a></h5>

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
<pre class="code-white language-php">
	<code class="imp-code language-php"><?php

    class SpecialComponent {

        #[Wire('id() === "specialId"')]
        private ?Collection $radios;
    }</code>
</pre>

<h5><a id="uid">uid()</a></h5>

Though this functions has no use cases yet, it can be useful in the future when you want to directly retrieve components with a certain uid on a concrete component instance.
