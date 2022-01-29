<h3 class="doc-title">Components</h3>

- [Introduction](#introduction)
    - [Server state](#server-state)
    - [Client state](#client-state)
    - [Reactivation](#reactivation)
	- [Available components](#registered_components)
- [Create custom components](#create_custom_components)
    - [Custom attributes](#custom_attributes)
    - [Client state](#client_synchronization)
    - [Server state](#server_synchronization)
    - [Client component](#client_component)
- [Learn more about components](#advanced_topics)

<h4><a id="introduction">Introduction</a></h4>

This article covers the basics of client implementation of components. In the "Learn about more components" section you will find further advanced topics related to components.

Note that everything related to client needs to have a complete working npm environment as described in the installation manual.

<h5><a id="server-state">Create custom component</a></h5>

Creating the client side of components is quite easy. Let's take again that javascript example from the server side documentation of components:

<pre class="imp-code code-white line-numbers language-js">
	<code class="language-js">import AbstractComponent from '../../../../vendor/impulsephp/impulsebundle/src/Resources/assets/js/impulse-bundle/Impulse/UI/Components/AbstractComponent';
    
    export default class Message extends AbstractComponent {
        constructor() {
            super();
            this.message = '';
        }

        setMessage(message) {
            this.message = message;
            if (this.isAttached()) {
                this.getHtmlComponent().innerHTML = this.message;
            }
        }

        getTemplate() {
            return '<div><%= component.message %></div>';
        }
    }
    
    window.Message = Message;</code>
</pre>

Above is the definition of a Message component that shall print a text inside a div container. 

A component is basically an object of a certain class with a load of properties and values. Since the Impulse PHP Framework will keep the state of each component after a request, the component will be serialized. This allows the framework to re-create all the states of the component in a subsequent request.

Since the basic serialization mechanism of PHP is not suitable for our needs, the components instead have a method called <span class="code-hint">getServerData</span> that creates an array of attributes and their values that will be used for later serialization. The following example is taken from the <span class="code-hint">Image</span> component class.

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
    
    class Image extends AbstractComponent 
    {
        protected string $src = '';
        protected ?string $alt = null;
        
        // other methods
        
        public function getServerData(): array
        {
            $data = parent::getServerData();
            $data['src'] = $this->src;
            $data['alt'] = $this->alt;
            return $data;
        }
    }</code>
</pre>

Basically the implementation of the Image class retrieves the server data from its parent class and adding both the src and the alt attribute and their values to the array.

<h5><a id="client-state">Client state</a></h5>

Analogous to the server state, the client (browser) will also have a stateful representation of the component handled by the Impulse Javascript Client engine. Since the component objects will stay alive as long as the browser tab is not destroyed, there is no need for serialization there. 

However, since the client needs the data from the server of all components aswell to create the DOM, there is another method called <span class="code-hint">getClientData</span>. It creates the data the same way as the <span class="code-hint">getServerData</span> method but only contains the data needed for the client component.

Imporant note: Please consider that getClientData might differ from the <span class="code-hint">getServerData</span> implementation since not all properties need to be populated to client to avoid disclosure of internals and sensitive data.

<h5><a id="reactivation">Reactivation</a></h5>

Components state are stored by the server and thus can later be reactivated to access them again in a subsequent request. The component will be reactivated will all their serialized properties and will reside
in tracking mode afterwards. The tracking mode means that it can record changes to the component objects that
can a) be send to the client to update the UI and b) consider the changes in the serialization process again after a request.

To record a change that is intended for the client, the <span class="code-hint">updateClientAttribute</span> method is used for. In the Image component again there is a setter for the src attribute that records changes that will later be populated to the client.

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
    
    class Image extends AbstractComponent 
    {
        protected string $src = '';
        // other attributes
        
        // other methods
        
        public function setSrc($src)
        {
            $this->src = $src;
            $this->updateClientAttribute($src, 'src');
        }
    }</code>
</pre>

The reactivation process of a component will be covered in a "Learn more" section at the bottom of this article. 

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


<h5><a id="client_synchronization">Client state</a></h5>

The main purpose is to show the message to the client and therefor there is a method called <span class="code-hint">getClientData</span> which returns an array with all contain informations of the component that are relevant for the client.

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

You also need to call the <span class="code-hint">getClientData</span> of the parent class (at least of AbstractComponent) since it contains necessary meta informations for the Impulse client engine.

If the client should also be informed about changes of the message property, we can use the <span class="code-hint">updateClientAttribute</span> method as mentioned in the introduction.

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

<h5><a id="server_synchronization">Server state</a></h5>

Applying the server state is straight forward and we need to just implement the <span class="code-hint">getServerData</span> method.

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

<h5><a id="client_component">Client component</a></h5>

Since the main focus of this article is the server side documentation, we will just provide a quick start component snippet for the client component. Please check out the component client documentation for more details.

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
	<code class="imp-code language-js">import AbstractComponent from '../../../../vendor/impulsephp/impulsebundle/src/Resources/assets/js/impulse-bundle/Impulse/UI/Components/AbstractComponent';
    
    export default class Message extends AbstractComponent 
    {
        constructor()
        {
        	super();
            this.message = '';
        }
        
        setMessage(message)
        {
        	this.message = message;
            if (this.isAttached()) {
            	this.getHtmlComponent().innerHTML = this.message;
        	}
        }
        
        create(parentComponent, childrenCount, childIndex)
        {
            let messageNode = this.client.createElementsFromStringWithUid(
            	this.client.renderTemplate(`<div><%= message %></div>`, { message: this.message }),
                this.getUid()
            );
            
            parentComponent.getParentWrapper(childIndex).append(messageNode);
        }
    }
    
    window.Message = Message;</code>
</pre>
