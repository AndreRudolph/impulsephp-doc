<h3 class="doc-title">Components</h3>

- [Introduction](#introduction)
- [Basics](#basics)
	- [Available components](#registered_components)
- [Create custom components](#create_custom_components)
    - [Custom attributes](#custom_attributes)
    - [Client synchronization](#client_synchronization)
    - [Server synchronization](#server_synchronization)
    - [Client component](#client_component)
- [Learn more about components](#advanced_topics)

<h4><a id="introduction">Introduction</a></h4>

Components are the key concept behind the idea of the Impulse PHP Framework. A component represents an object that is (mostly) stored on both server and client side. At the client side, a component is a rendered HTML representation of the component and all of its descendants. At the server side, the state of the component is stored in the users session in order to reactivate the components state whenever it will be accessed at server side again.

Once a component is stored at server side, it remains in an updateable state which tracks changes to the object state and stores them back in the session. The changes are later synchronized with the client to update the appeareance of the HTML representation. 

All of these (and even more) concepts and mechanisms are covered by this documentation and further references at the bottom the page.

<h4><a id="basics">Basics</a></h4>

<h5><a id="registered_components">Available components</a></h5>

For a complete list of currently registered components you can run the following command in your projects root directory:

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
  	<code class="language-bash">php bin/console debug:impulse:components</code>
  </pre>
</div>

A shortened extract of the output might look as the following:

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
  <pre class="code-white imp-code line-numbers language-bash">
  	<code class="language-bash"> ------------------------- ----------------------------------------------------- 
      Alias                     Class                                                
     ------------------------- ----------------------------------------------------- 
      a                         Impulse\ImpulseBundle\UI\Components\A                
      impulse:a                 Impulse\ImpulseBundle\UI\Components\A                
      blockquote                Impulse\ImpulseBundle\UI\Components\Blockquote       
      impulse:blockquote        Impulse\ImpulseBundle\UI\Components\Blockquote       
      br                        Impulse\ImpulseBundle\UI\Components\Br               
      impulse:br                Impulse\ImpulseBundle\UI\Components\Br               
      button                    Impulse\ImpulseBundle\UI\Components\Button           
      impulse:button            Impulse\ImpulseBundle\UI\Components\Button           
      carousel                  Impulse\ImpulseBundle\UI\Components\Carousel         
      impulse:carousel          Impulse\ImpulseBundle\UI\Components\Carousel         
      ...           
      upload                    Impulse\ImpulseBundle\UI\Components\Upload           
      impulse:upload            Impulse\ImpulseBundle\UI\Components\Upload           
      window                    Impulse\ImpulseBundle\UI\Components\Window           
      impulse:window            Impulse\ImpulseBundle\UI\Components\Window                             
     ------------------------- -----------------------------------------------------</code>
  </pre>
</div>

You wonder why every component is listed twice? The reason behind this is covered in the <a href="#component_contexts">Component contexts</a> section below.

Available components are discovered automatically and you don't have to register any component class by yourself as long as you follow the conventions. The convention is that all classes within the UI/Components/ directory which implement the ComponentContract interface are registered as components automatically.

<h5><a id="component_contexts">Component contexts</a></h5>

The ComponentClassRegistry is the lookup class in which all discovered components are registered. Remember the following command to list all available components.

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
  	<code class="language-bash">php bin/console debug:impulse:components</code>
  </pre>
</div>

As you maybe noticed each component is registered at least twice. For example the button component is registered as 'button' and 'impulse:button'. The reason behind this is that the Impulse framework offers you the opportunity to have multiple button components registered.

Consider the following scenario:
App/
	UI/
    	Components/
        	Button.php

In the example above in the App is another button component. The component scan discovery overwrites non contextual aliases with the last find. According to this, the following button

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
        &lt;button /&gt;
    &lt;/impulse&gt;</code>
    </pre>
</div>

will be bound to the app button component class. However, you can also explicitly use the button implementation that is provided by the Impulse framework with a contextual prefix.

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
        &lt;impulse:button /&gt;
    &lt;/impulse&gt;</code>
  </pre>
</div>


<h5><a id="create_own_components">Create custom components</a></h5>

Impulse is designed to provide programmers the possibility to create their own components for their very specific needs or to even share with other users of the Impulse framework. As previousely mentioned, a component can be either very basic and atomic components like a textbox or a label or can be even more sophisticated like even a wysiwyg editor. In this overall documentation part we will create a full working component together.

<h5><a id="custom_attributes">Custom attributes</a></h5>

Suppose you want to create a component class, lets call it **_Message_**, with a message that shall be displayed at the client. You can simply add a message property along with its getter and setter method.

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
    
    class Message extends AbstractComponent 
    {
        private string $message = 'Hello World';
        
        public function setMessage(string $message): void
        {
        	$this->message = $message;
        }
        
        public function getMessage(): string
        {
        	return $this->message;
        }
    }</code>
</pre>


<h5><a id="client_synchronization">Client synchronization</a></h5>

The main purpose is to show the message to the client and therefor there is a method called **_getClientData_** which returns an array with all contain informations of the component that are relevant for the client.

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
    
    class Message extends AbstractComponent 
    {    
        // ... message property and its getter and setter
        
        public function getClientData(): array
        {
        	$data = parent::getClientData();
            $data['message'] = $this->message;
            return $data;
        }
    }</code>
</pre>

You also need to call the getClientData of the parent class (at least of AbstractComponent) since it contains necessary meta informations for the Impulse client engine.

It might happen that, maybe by some circumstances, changes to the message property should be synchronized with the client component object. For this purpose you have to call the **_updateClientAttribute_** method inside the setter. 

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
    
    class Message extends AbstractComponent
    {
        // ... 
        
        public function setMessage(string $message): void
        {
        	$this->message = $message;
            $this->updateClientAttribue($message, 'message');
        }
    }</code>
</pre>

Basically components have an internal property called **_info_** which is designed as a tracking container for component changes.

<h5><a id="server_synchronization">Server synchronization</a></h5>

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
    
    class Message extends AbstractComponent 
    {
        // ... message property and its getter and setter
        
        public function getServerData(): array
        {
        	$data = parent::getServerData();
            $data['message'] = $this->message;
            return $data;
        }
    }</code>
</pre>

<h4><a name="advanced_topics">Learn more about components</a></h4>

<ul class="unstyled-list">
  <li><a data-target-menu-item="component_contexts">Component contexts</a></li>
  <li><a data-target-menu-item="component_lifecycle" class="text-muted">Component lifecycle</a></li>
  <li><a data-target-menu-item="component_service_wiring">Components Service Wiring</a></li>
  <li><a data-target-menu-item="components_el_wiring">Components Expression Language Based Wiring</a></li>
  <li><a data-target-menu-item="components_volatile">Volatile components</a></li>
  <li><a data-target-menu-item="component_guards" class="text-muted">Component guards</a></li>
</ul>
