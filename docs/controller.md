---
layout: post
title: Start of Documentation
published: true
---
<h3 class="doc-title">Controllers</h3>

- [Introduction](#introduction)
- [Basics](#basics)
    - [Controller class](#controller-class)
    - [Access components](#access-components)
    - [Load views](#load-views)
    - [Event listening](#listen-to-events)
- [Advanced topics](#advanced-topics)

<a name="introduction"></a>

Controllers in Impulse PHP Framework are not exactly the same as you might be used with other frameworks. In most other frameworks among other responsibilites, a controller is that instance that decides which view must be rendered. However, in impulse the exact opposite is the case that a view binds a concrete controller class as you have already explored in the **_Views_** section.

<h4><a id="basics">Basics</a></h4>

<h5><a id="controller-class">Controller class</a></h5>

A minimalistic controller class is an empty class that extends the AbstractController class which is provided by the framework. The base class provides some useful functionalities that will be covered in a later section.

As previousely mentioned, a controller will be rarely called directly from the frameworks user. 

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
		&lt;window bind="App\Controller\AppController" /&gt;
    &lt;/impulse&gt;</code>
</pre>

The <span class="highlightText">bind</span> attribute defines which controller will be executed by the framework. The following controller example is the bare minimum.  

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

This example controller above would do nothing. For doing initial tasks you may override the afterCreate method from the AbstractController. The <span class="highlightText">afterCreate</span> represents the entry point of the controller and will be called automatically after the view has been rendered.

<h5><a name="access-components">Access components</a></h5>

The main purpose of controllers in Impulse is that they can directly interact with all components which have been created by views in an object orientated way. There are different strategies how these components can be accessed within a controller. However, one option is by wiring by convention by naming properties with the related component ids.

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

We have created a textbox with the id <span class="highlightText">tb</span>. This component is - by convention - wireable for a controller and can be accessed via controller property.

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

In the example above, once the controller was created, the afterCreate method will be called automatically (see chapter before) and the textbox value will be set to Hello World!.

<h5><a id="load-views">Load views</a></h5>

The more complex the application gets the more it should be divided in smaller parts (single responsiblity principle). For exactly this purpose you can easily load views inside your controller. The following example shows how this works.

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
		&lt;window id="wndMain" bind="App\Controller\AppController" /&gt;
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

<h5><a id="listen-to-events">Event listening</a></h5>

Controllers are designed to work as event listeners to listen to events occur at client side. The framework internally maps client events to AJAX requests that will be send to the server and thus delegated to the correct controller instance. How this in detail works is decscribed in the **_Event mapping_** section. However, the example belows demonstrates how event listeners are registered.

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
		&lt;window bind="App\Controller\AppController"&gt;
			&lt;textbox id="tbName" /&gt;
			&lt;button id="btnGreet" /&gt;
			&lt;span id="lbGreet" /&gt;
		&lt;/window&gt;
	&lt;/impulse&gt;</code>
  </pre>
</div>

The controller contains method that is annotated with the **_Listen_** annotation.

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
    use Impulse\ImpulseBundle\Events\Events;
	use Impulse\ImpulseBundle\Controller\Annotations\Listen;
	use Impulse\Bundles\ImpulseBundle\UI\Components\Span;
	use Impulse\Bundles\ImpulseBundle\UI\Components\Textbox;

	class AppController extends AbstractController
	{
		private ?Textbox $tbName = null;
		private ?Span $lbGreet = null;

        #[Listen(event: Events::CLICK, component: 'btnGreet')]
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


<h4><a id="advanced_topics">Learn more about components</a></h4>

<ul class="unstyled-list">
  <li><a data-target-menu-item="controller_serialization" class="text-muted">Serialization</a></li>
</ul>
