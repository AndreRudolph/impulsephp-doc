<h3 class="doc-title">WebsocketBundle installation</h3>

- [Introduction](#introduction)
- [Installation](#installation)
- [Basics](#basics)
    - [Controller class](#controller-class)
    - [Access components](#access-components)
    - [Load views](#load-views)
    - [Event listening](#listen-to-events)
- [Advanced topics](#advanced-topics)

<a name="introduction"></a>

Websockets is basically a network protocol allowing bidirectional communication between client and server. Once connected, the connection is usually kept open until the client manually closes the connection by refreshing or closing the browser tab. The main difference between conventional HTTP communication is, that the client does not need to request for an update or a notification. Instead, the server is able to continousely send updates to the client. 

With that being sad, by processing a client request (such as sending a message in a chat room) other available clients can be informed about that message too and display it **_without explicitly_** requesting it.


<h4><a id="basics">Installation</a></h4>

Installation of the ImpulseWebsocketBundle is pretty easy by adding it to composer require command.

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
  <pre class="code-white imp-code line-numbers language-shell">
	<code class="language-bash">composer require impulsephp/impulsewebsocketbundle</code>
  </pre>
</div>

If not provided automatically, you need to create the config file impulse_websockets.yaml inside the config/packages directory for the websocket settings. The file contains the following default settings:

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
  <pre class="code-white imp-code line-numbers language-yaml">
	<code class="language-yaml">impulse_websockets:
		enabled: true
		protocol: 'ws'
		host: 'localhost'
		port: 7777</code>
  </pre>
</div>

After that, you need to extend the App's ImpulseController by the one provided by the ImpulseWebsocketBundle.

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

	class ImpulseController extends \Impulse\ImpulseWebsocketBundle\Controller\ImpulseController
	{

	}</code>
  </pre>
</div>

The last step is to include the js ImpulseWebsocketBundle by copying it from the Bundle and place it inside the assets/js directory and import it in the app.js

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
  <pre class="code-white line-numbers language-js">
  	<code class="imp-code language-js">import './impulse-bundle/ImpulseCore';
	import './impulse-bundle/ImpulseComponents';
	import './impulse-websocket-bundle/ImpulseWebsocketBundleEntry';

	import './app/homepage/AppComponents';

	import './../scss/app.scss';

    document.addEventListener("DOMContentLoaded", function(event) {
        let app = new App();
        app.run();
    });</code>
  </pre>

------------

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
  	use Impulse\ImpulseBundle\Controller\AbstractController;

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
	use Impulse\ImpulseBundle\Controller\AbstractController;
	use Impulse\ImpulseBundle\Execution\Events\Event;
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
	use Impulse\ImpulseBundle\Controller\AbstractController;
	use Impulse\ImpulseBundle\Execution\Events\Event;
	use Impulse\ImpulseBundle\UI\Components\Window;

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

Controllers are designed to work as event listeners to listen to events occur at client side. The framework internally maps client events to AJAX requests that will be send to the server and thus delegated to the correct controller instance. How this in detail works is decscribed in the **_Event mapping_** section. However, the example below demonstrates how event listeners can registered by annotation.

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

The controller contains a method that is annotated with the **_Listen_** annotation which needs the event it listens to and the component on which the event shall be registered. For registering the same event listener to multiple components, you can use the **_components_** parameter of the Listen annotation instead.

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
	use Impulse\ImpulseBundle\Controller\AbstractController;
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

Though this is a clean way to define event listeners, it has one drawback. It can't (currently) be registered dynamically for components which ids are not set or not known. For this purpose you can register event listeners manually to components.

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
    use Impulse\ImpulseBundle\Execution\Events\Event;
	use Impulse\ImpulseBundle\Controller\Annotations\Listen;
	use Impulse\Bundles\ImpulseBundle\UI\Components\Span;
	use Impulse\Bundles\ImpulseBundle\UI\Components\Textbox;

	class AppController extends AbstractController
	{
		private ?Textbox $tbName = null;
		private ?Span $lbGreet = null;
        private ?Button $btnGreet = null;
        
        public function afterCreate(Event $event)
        {
        	parent::afterCreate($event);
            $this->btnGreet->addEventListener(Events::CLICK, $this, 'greet');
        }

		public function greet()
		{
			$greeting = $this->tbName->getValue();
			$this->lbGreet->setValue($greeting);
		}
	}</code>
  </pre>
</div>

The outcome and functionality of the code above is the same as in the previous example with the Listen annotation. On line 20, the first argument is the event that will be listened to. The second paratmer is the actual controller instance that will be used as event listener while the third paramter declares the method that will be executed once the event is fired.

<h4><a id="advanced_topics">Learn more about controllers</a></h4>

<ul class="unstyled-list">
  <li><a id="controller_serialization" data-target-menu-item="controller_serialization">Serialization of controller</a></li>
</ul>
