<h3 class="doc-title">Components</h3>

- [Introduction](#introduction)
- [Basics](#basics)
	- [List available components](#registered_components)
	- [Client-server-synchronization](#client_server_synchronization)
    - [Create custom components](#create_custom_components)
    - [Component contexts](#component_contexts)
- [Learn more about components](#advanced_topics)

<h4><a id="introduction">Introduction</a></h4>

Components are the key concept behind the idea of the Impulse PHP Framework. A component represents an object that is (mostly) stored on both server and client side. At the client side, a component is a rendered HTML representation of the component and all of its descendants. At the server side, the state of the component is stored in the users session in order to reactivate the components state whenever it will be accessed at server side again.

Once a component is stored at server side, it remains in an updateable state which tracks changes to the object state and stores them back in the session. The changes are later synchronized with the client to update the appeareance of the HTML representation. 

All of these (and even more) concepts and mechanisms are covered by this documentation and further references at the bottom the page.

<h4><a id="basics">Basics</a></h4>

<h5><a id="registered_components">List available components</a></h5>

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

<h5><a id="client_server_synchronization">Client-server-synchronization</a></h5>

The main purpose of the components is to store both the component at the client and at the server side. The logical conclusion is that server-side changes to a component that would affect its appeareance or behavior must be synchronized with the client (browser).

To achieve this, most of the setters for attributes (e.g. setHeight, setVisible, etc.) are observed for changes. This means whenever you set the height of the component after it was created and populated in a request before, the internal server side state of the components gets updated and the client receives an update aswell.

<h5><a id="create_own_components">Create custom components</a></h5>

Impulse is designed to provide programmers the possibility to create their own components for their very specific needs or to even share with other users of the Impulse framework. As previousely mentioned, a component can be either very basic and atomic components like a textbox or a label or can be even more sophisticated like even a wysiwyg editor.  

To support you, you can create a skeleton component with the following command.

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
		<code class="language-bash">php bin/console make:impulse:component</code>
	</pre>
</div>


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

<a name="advanced_topics">Learn more about components</a>

<ul class="unstyled-list">
  <li><a data-target-menu-item="component_lifecycle" class="text-muted">Component lifecycle</a></li>
  <li><a data-target-menu-item="component_service_wiring">Components Service Wiring</a></li>
  <li><a data-target-menu-item="components_el_wiring">Components Expression Language Based Wiring</a></li>
</ul>
