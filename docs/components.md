# Components

- [Introduction](#introduction)
	- [Registered components](#registered_components)
- [Basics](#basics)
	- [Client-server-synchronization](#client_server_synchronization)
    - [Creating own components](#create_own_components)
- [Advanced topics](#advanced_topics)
	- [Component lifecycle](#component_lifecycle)
    - [Wiring services into components](#wiring)
    - [Component contexts](#component_contexts)

<a name="introduction"></a>
### Introduction

Components are the heart of the Impulse PHP Framework. Almost every native html tag is considered as a component itself. Unlike 'normal' (ajax based) web application such componnets have a state on client as well on server side. 

Each component has a set of server side managed attributes that are directly stored within the users session. For example width, height, custom attributes, etc. However, most of the components do have more attributes, e.g. 'value' for textbox or 'label' for button components. 

Like the html representatives, each component has exactly one parent (except root) and zero to any number of childs. When the session gets persisted or updated, the parent-/child relationships will also be stored.

<a name="registered_components"></a>
<h3>Registered components</h3>

For a complete list of currently registered components you can run the following command:

<pre class="imp-code line-numbers language-shell">
<code class="language-bash">php bin/console debug:impulse:components</code>
</pre>

Available components are discovered automatically and you don't have to register any component class by yourself as long as you follow the conventions.

<a name="basics"></a>
### Basics

<a name="client_server_synchronization"></a>
<h3>Client-server-synchronization</h3>

The main purpose of the components is to store both the component at the client and at the server side. The logical conclusion is that server-side changes to a component that would affect its appeareance or behavior must be synchronized with the client (browser).

To achieve this, most of the setters for attributes (e.g. setHeight, setVisible, etc.) are observed for changes. This means whenever you set the height of the component after it was created and populated in a request before, the internal server side state of the components gets updated and the client receives an update aswell.

<a name="create_own_components"></a>
<h3>Creating own components</h3>

<a name="advanced_topics"></a>
### Advanced topics

<a name="component_lifecycle"></a>
<h3>Component lifecycle</h3>

<a name="wiring"></a>
<h3>Wiring services into components</h3>

Components are also capable of being wired with services via the dependency injection container. All you have to do is let your component implement the WiringAware interface which is just a marker interface. 

<a name="component_contexts"></a>
<h3>Component contexts</h3>

The ComponentClassRegistry is the lookup class in which all discovered components are registered. Remember the following command to list all available components.

<pre class="imp-code line-numbers language-shell">
<code class="language-bash">php bin/console debug:impulse:components</code>
</pre>

As you maybe noticed each component is registered at least twice. For example the button component is registered as 'button' and 'impulse:button'. The reason behind this is that the Impulse framework offers you the opportunity to have multiple button components registered. 

Consider the following scenario:
App/
	UI/
    	Components/
        	Button.php
            
In the example above in the App is another button component. The component scan discovery overwrites non contextual aliases with the last find. According to this, the following button

<pre class="line-numbers language-markup">
<code class="imp-codelanguage-php language-markup">&lt;impulse&gt;
    &lt;button /&gt;
&lt;/impulse&gt;</code>
</pre>

will be bound to the app button component class. However, you can also explicitly use the button implementation that is provided by the Impulse framework with a contextual prefix.

<pre class="line-numbers language-markup">
<code class="imp-codelanguage-php language-markup">&lt;impulse&gt;
    &lt;impulse:button /&gt;
&lt;/impulse&gt;</code>
</pre>