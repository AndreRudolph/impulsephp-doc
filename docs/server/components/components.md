<h3 class="doc-title">Components</h3>

- [Introduction](#introduction)
- [Creating components](#creating-components)
    - [Server state](#server-state)
    - [Client state](#client-state)
    - [Reactivation](#reactivation)
	- [Available components](#registered_components)
- [Create custom components](#create_custom_components)
    - [Client state](#client_synchronization)
    - [Server state](#server_synchronization)
    - [Client component](#client_component)
- [Custom attributes](#custom-attributes)
- [Template components](#template-components)
- [Event listening](#event-listening)
- [Learn more about components](#advanced_topics)

<h4><a id="introduction">Introduction</a></h4>

A component is an object with state and event listeners attached to it. It will be managed
by the server and rendered by the client. 

<h4><a id="creating-components">Creating components</a></h4>

Components should extend the AbstractComponent or one of its descendant classes. An easy
way is by using template based components.

<pre class="imp-code code-white language-markup">
<code class="language-markup">&lt;div bind="App\UI\Components\Counter"&gt;
    &lt;textbox id="tbCount" value="0" /&gt;
    &lt;button id="btnIncrement" label="Increment" on:click="increment" /&gt;
&lt;/div&gt;</code>
</pre>

<pre class="imp-code code-white language-php">
<code class="language-php"><?php

#[Template('app/components/counter.html.twig')]
class Counter extends Div implements AfterCreateChilds
{
    private Textbox $tbCounter;
    
    public function increment(): void
    {
        $this->tbCounter->setValue($this->tbCounter->getValue() + 1);
    }
}</code>
</pre>

You can then use this component in your templates:

<pre class="imp-code code-white language-markup">
<code class="language-markup">&lt;div&gt;
    &lt;counter />
&lt;/div&gt;</code>
</pre>

<pre class="imp-code code-white language-php">
<code class="language-php"><?php

class Text extends Div {
    private string $text = '';

    public function getClientData(): array {
        $data = parent::getClientData();
        $data['text'] 
    }
}</code>
</pre>


<h5><a id="server-state">Server state</a></h5>

A component is basically an object of a certain class with a load of properties and values. Since the Impulse PHP Framework will keep the state of each component after a request, the component will be serialized. This allows the framework to re-create all the states of the component in a subsequent request.

Since the basic serialization mechanism of PHP is not suitable for our needs, the components instead have a method called <span class="code-hint">getServerData</span> that creates an array of attributes and their values that will be used for later serialization. The following example is taken from the <span class="code-hint">Image</span> component class.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php

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

Components state are stored by the server and thus can later be reactivated to access them again in a subsequent request. The component will be reactivated with all their serialized properties and will reside
in tracking mode afterwards. The tracking mode means that it can record changes to the component objects that
can a) be send to the client to update the UI and b) consider the changes in the serialization process again after a request.

To record a change that is intended for the client, the <span class="code-hint">updateClientAttribute</span> method is used for. In the Image component again there is a setter for the src attribute that records changes that will later be populated to the client.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php

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

<pre class="code-white imp-code language-shell">
<code class="language-bash">php bin/console debug:impulse:components</code>
</pre>

A shortened extract of the output might look as the following:

<pre class="code-white imp-code language-bash">
<code class="language-bash">------------------------- ----------------------------------------------------- 
Alias                     Class                                                
------------------------- ----------------------------------------------------- 
a                         Impulse\ImpulseBundle\UI\Components\A                
impulse:a                 Impulse\ImpulseBundle\UI\Components\A                  
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

You wonder why every component is listed twice? The reason behind this is covered in the <a href="#component_contexts">Component contexts</a> section below.

Available components are discovered automatically and you don't have to register any component class by yourself as long as you follow the conventions. The convention is that all classes within the UI/Components/ directory which implement the ComponentContract interface are registered as components automatically.

<h5><a id="create_own_components">Create custom components</a></h5>

Impulse is designed to provide programmers the possibility to create their own components for their very specific needs or to even share with other users of the Impulse framework. As previousely mentioned, a component can be either very basic and atomic components like a textbox or a label or can be even more sophisticated like even a wysiwyg editor. In this overall documentation part we will create a full working component together.

Suppose you want to create a component class, lets call it **_Message_**, with a message that shall be displayed at the client. You can simply add a message property along with its getter and setter method.

<pre class="imp-code code-white language-php code-xl">
<code class="language-php"><?php

class Message extends AbstractComponent 
{
    private string $message = 'Hello World';
    
    public function __construct() 
    {
        parent::__construct();
        $this->uiClass = 'Message';
    }
    
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

The property <span class="code-hint">uiClass</span> later indicates which javascript class will be associated with that component.

<h5><a id="client_synchronization">Client state</a></h5>

The main purpose is to show the message to the client and therefor there is a method called <span class="code-hint">getClientData</span> which returns an array with all contain informations of the component that are relevant for the client.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php

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

<pre class="imp-code code-white language-php">
<code class="language-php"><?php

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

<pre class="imp-code code-white language-php">
<code class="language-php"><?php

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

<pre class="imp-code code-white language-js">
<code class="language-js">import { AbstractReactComponent } from '@impulsephp/client-ts';

export class Message extends AbstractReactComponent {
    private message: string;

    constructor(props) {
        super(props);
        this.message = props.message;
    }
    
    getTemplate(attributes) {
        return (
            &lt;div {...attributes}&gt;{{ this.message }}&lt;/div&gt;
        );
    }
}

window.Message = Message;</code>
</pre>

<h5><a id="custom-attributes">Custom attributes</a></h5>

Custom attributes are usefuly whenever you want to enrich a component with very specific informations for later work with these attributes. Consider you want to associate a button with an entity id of a database record. You can simply achieve this by calling the <span class="code-hin">setCustomAttribute</span> method of that component.

<pre class="imp-code code-white language-php code-xl">
<code class="language-php"><?php
    
namespace App\Controller;
use Impulse\ImpulseBundle\Controller\AbstractController;

class AppController extends AbstractController
{
    private Button $btnDeleteUser;
    
    public function afterCreate() 
    {
    	$this->btnDeleteUser->setCustomAttribute('userId', 1);
    }
    
    #[Listen(event: 'click', component: 'btnDeleteUser')]
    public function onDeleteUser(Event $event)
    {
    	$userId = $event->getTarget()->getCustomAttribute('userId'); // => will be 1
    }
}</code>
</pre>

<h5><a id="template-components">Template components</a></h5>

Another great approach of creating complex and reusable components are template components. A template component is basically a component class which structure is defined by a template (view). The template that is used for building up the component is defined by the attribute (annotation) <span class="code-hint">#[Template]</span>. As an argument it contains a path to the template file. 

The example below shows how template components work with a greeting example.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php

#[Template('app/components/greeting.html.twig')]
class Greeting extends Div implements AfterCreateChilds
{
    private Textbox $tbName;
    private Button $btnGreet;
    
    public function afterCreateChilds(): void
    {
        $this->btnGreet->addEventListener(Events::CLICK, $this, 'onGreet');
    }
    
    public function onGreet(): void
    {
            
    }
}</code>
</pre>

<pre class="imp-code code-white language-markup">
<code class="language-markup">&lt;div bind="App\UI\Components\Greeting"&gt;
    &lt;textbox id="tbName" placeholder="Enter your name here" /&gt;
    &lt;button id="btnGreet" label="Greet" /&gt;
&lt;/div&gt;</code>
</pre>

This approach is a good way to encapsulate view based logic in components rather than in controllers.

<h4><a id="event-listening">Event listening</a></h4>

How components are registered as event listeners can be read in the Events documentation.
