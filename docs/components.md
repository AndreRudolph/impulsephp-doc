<h1 class="doc-title">Components</h1>

- [Introduction](#introduction)
- [Basics](#basics)
	- [Registered components & discovery](#registered_components)
	- [Client-server-synchronization](#client_server_synchronization)
    - [Creating own components](#create_own_components)
- [Learn more about components](#advanced_topics)

<a name="introduction"></a>
### Introduction

Components is the key concept behind the Impulse PHP Framework because literally everything is based on that. A component represents an object that itself is rendered as a HTML representation in the browser via the Impulse javascript engine. Components can be either trivial and atomar (e.g. a textbox) or even more complex like a caroussel component. Almost every native html tag is considered as a component itself. Unlike 'normal' (ajax based) web application such componnets have a state on client as well on server side.

Each component (and of course its state) is stored within the user's session. This is required to keep the state of a component alive as long as the component resides in the DOM. Every component should inherit directly or indirectly form the AbstractComponent since it implements the necessary basics to have a working component. Once a component has been created either on server side as well on client side, the component resides in an updatable state. This means that most of the components attributes are synchronized with the client like e.g. width, height (defined in AbstractComponent) or value for textbox components. Whenever you update on of the synchronizable attributes, the changes are directly recorded and part of the response of the server.

Like the html representatives, each component has exactly one parent (except root) and zero to any number of childs. When the session gets persisted or updated, the parent-/child relationships will also be stored.

<a name="basics"></a>
### Basics

<a name="registered_components"></a>
<h3>Registered components &amp; discovery</h3>

For a complete list of currently registered components you can run the following command:

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

Available components are discovered automatically and you don't have to register any component class by yourself as long as you follow the conventions. The convention is that all classes within the UI/Components/ directory which implement the HtmlBasedComponentContract are registered as components automatically.

<a name="client_server_synchronization"></a>
<h3>Client-server-synchronization</h3>

The main purpose of the components is to store both the component at the client and at the server side. The logical conclusion is that server-side changes to a component that would affect its appeareance or behavior must be synchronized with the client (browser).

To achieve this, most of the setters for attributes (e.g. setHeight, setVisible, etc.) are observed for changes. This means whenever you set the height of the component after it was created and populated in a request before, the internal server side state of the components gets updated and the client receives an update aswell.

<a name="create_own_components"></a>
<h3>Creating own components</h3>

You can also extend the framework or your app with your own components. You can also created more complex and customized components. You can create everything from small, atomic components (e.g. span, textarea) to more complex components like a Breadcrumb or wysiwyg editor.

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


<a name="component_contexts"></a>
<h3>Component contexts</h3>

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

<a name="advanced_topics"></a>
### Learn more about components
<ul class="unstyled-list">
  <li><a data-target-menu-item="component_lifecycle">Component lifecycle</a></li>
  <li><a data-target-menu-item="component_service_wiring">Components Service Wiring</a></li>
  <li><a data-target-menu-item="components_el_wiring">Components Expression Language Based Wiring</a></li>
</ul>
