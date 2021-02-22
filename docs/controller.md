---
layout: post
title: Start of Documentation
published: true
---


<h1 class="doc-title">Controllers</h1>

- [Introduction](#introduction)
- [Basic Controllers](#basic-controllers)
    - [ Defining Controllers](#defining-controllers)
    - [Wire components](#wire-components)
    - [Nested controllers](#nested-controllers)
    - [Controllers as event listener](#controller-eventlistener)
    - [Explicit Controller Event Listener registration](#explicit-controllerEventListener)
- [Advanced topics](#advanced-topics)
    - [Concept of serialization](#concept-serialization)
    - [Excluding attributes from serialization]

<a name="introduction"></a>
# Introduction

A controller is the main component that is responsible for handling (and thus managing) user interactions. Unlike other frameworks, a controller in the Impulse PHP Framework itself does not directly return views. Instead, a view template specifies the related controller that will be rendered.

<a name="basis-controller">
## Basic controllers

#### Defining controllers
Defining a controller is not very complicated. A controller is never called directly from the frameworks user. Instead a controller will be bound to a certain template.

<!--<pre class="code-white line-numbers language-markup">-->
<div class="code-header">
	<div class="container-fluid">
		<div class="row">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
    </div>
</div>
<pre class="code-white line-numbers language-markup">
	<code class="imp-code language-markup">&lt;impulse&gt;
		&lt;window apply="App\Controller\AppController" /&gt;
    &lt;/impulse&gt;</code>
</pre>

The <span class="highlightText">apply</span> attribute defines which controller will be executed by the framework. The following controller example is the bare minimum.  

<div>
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
  	namespace App\Controller;
  	use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;

  	class AppController extends AbstractController
  	{

  	}</code>
  </pre>
</div>

This example controller above would do nothing. For doing initial tasks you may override the afterCreate method from the AbstractController. The <span class="highlightText">afterCreate</span> represents the entry point of the controller and will be called automatically when the view was rendered.

<a name="wire-components" />
#### Wire components
Components can be wired directly into the controller by naming conventions. They must be named exactly like their id in the view.

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
            <div class="button red"></div>
          	<div class="button yellow"></div>
          	<div class="button green"></div>
        </div>
    </div>
  </div>
  <pre class="code-white line-numbers language-markup">
	<code class="imp-code language-markup">&lt;impulse&gt;
    	&lt;window&gt;
        	&lt;textbox id="tb" /&gt;
		&lt;/window&gt;
	&lt;/impulse&gt;</code>
  </pre>
</div>

We have created a textbox with the id <span class="highlightText">tb</span>. This component is - by naming convention - wireable for a controller.

<div>
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
	namespace App\Controller;
	use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;
	use Impulse\Bundles\ImpulseBundle\Execution\Events\Event;
	use Impulse\ImpulseBundle\UI\Components\Textbox;

	class AppController extends AbstractController
	{
		private ?Textbox $tb = null;

		public function afterCreate(Event $event)
		{
			parent::afterCreate($event);
			$this->tb->setValue('Hello world!');
		}
	}</code>
  </pre>
</div>

After the execution is finished, the value of the textbox should be <span class="highlightText">Hello world!</span>.

#### Nested controllers
Another main feature is view and therefor controller nesting. See more for view nesting in the views documentation. This section will only cover controller nesting. This can be achieved by importing a template within the executed controllers method. Below is an example of controller nesting.

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
            <div class="button red"></div>
          	<div class="button yellow"></div>
          	<div class="button green"></div>
        </div>
    </div>
  </div>
  <pre class="code-white line-numbers language-markup">
  	<code class="imp-code language-markup">&lt;impulse&gt;
		&lt;window id="wndMain" apply="App\Controller\AppController" /&gt;
	&lt;/impulse&gt;</code>
  </pre>
</div>

<div>
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
	namespace App\Controller;
	use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;
	use Impulse\Bundles\ImpulseBundle\Execution\Events\Event;
	use Impulse\Bundles\ImpulseBundle\UI\Components\Window;

	class AppController extends AbstractController
	{
		private ?Window $wndMain = null;

		public function handleEvent(Event $event)
		{
			$this->importView('importedView.imp', $this->wndMain);
		}
	}</code>
  </pre>
</div>

Line 13 is the important line. The <span class="highlightText">importView</span> method is called and takes two arguments. The first one is the view that will be imported and the second argument is the component that will be the direct parent of the root component of the imported view.

By default this will remove all childs of the given parent component internally. To append instead of removing all childs, you can set an optional third parameter with <i>true</i> as value.

<a name="controller-eventlistener"></a>
#### Controllers as event listener
Just defining a controller is by far not the only supported feature. As you maybe noticed, controllers can also work as event listeners as well. Consider the following example.

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
            <div class="button red"></div>
          	<div class="button yellow"></div>
          	<div class="button green"></div>
        </div>
    </div>
  </div>
  <pre class="code-white line-numbers language-markup">
  	<code class="imp-code language-markup">&lt;impulse&gt;
		&lt;window apply="App\Controller\AppController"&gt;
			&lt;textbox id="tbName" /&gt;
			&lt;button id="btnGreet" /&gt;
			&lt;span id="lbGreet" /&gt;
		&lt;/window&gt;
	&lt;/impulse&gt;</code>
  </pre>
</div>

The controller contains an event listener method.

<div>
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
	namespace App\Controller;
	use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;
	use Impulse\Bundles\ImpulseBundle\Execution\Events\Event;
	use Impulse\Bundles\ImpulseBundle\Controller\Annotations\Listen;
	use Impulse\Bundles\ImpulseBundle\UI\Components\Span;
	use Impulse\Bundles\ImpulseBundle\UI\Components\Textbox;

	class AppController extends AbstractController
	{
		private ?Textbox $tbName = null;
		private ?Span $lbGreet = null;

		/**
		* @Listen(component="btnGreet", event="click")
		*/
		public function onClick(Event $event)
		{
			$greeting = $this->tbName->getValue();
			$this->lbGreet->setValue($greeting);
		}
	}</code>
  </pre>
</div>

The controller has been extended by two new attributes and an <span class="highlightText">onClick</span> method. The name of the attributes correlate with their id in the view template. Thus, the framework can automatically inject these component objects to the controller. The <span class="highlightText">onClick</span> method is annotated with a <span class="highlightText">@Listen</span> annotation. This is a marker for the framework to automatically register an event listener.

The annotation consists of two parameters. The component parameter requires one component id or a comma separated list of components ids for which the event listener will be registered. The second parameter defines the event type that must be triggered on the client side to fire the execution of the event listener.

<a name="explicit-controllerEventListener"></a>
#### Explicit Controller Event Listener registration

<a name="advanced-topics"></a>
## Advanced topics

<a name="concept-serialization"></a>
#### Concept of serialization

Each controller will be serialized to the user session after the execution lifecycle has been completed. This is neccessary because they have to be reactived if required. All of the controllers attributes will be serialized by default.

Component objects will not be directly serialized within the controller. The serialized controller only contains the uuids of the attributes which held component references.
