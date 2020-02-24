# Components

- [Introduction](#introduction)
	- [Registered components](#registered_components)

<a name="introduction"></a>
### Introduction

Components are the heart of the Impulse PHP Framework. Almost every native html tag is considered as a component itself. Unlike 'normal' (ajax based) web application such componnets have a state on client as well on server side. 

Each component has a set of server side managed attributes that are directly stored within the users session. For example width, height, custom attributes, etc. However, most of the components do have more attributes, e.g. 'value' for textbox or 'label' for button components. 

Like the html representatives, each component has exactly one parent (except root) and zero to any number of childs. When the session gets persisted or updated, the parent-/child relationships will also be stored.



<a name="registered_components"></a>
<h3>Registered components</h3>

For a complete list of currently registered components you can run the following command:

<pre class="imp-code line-numbers language-bash">
<code class="language-bash">php bin/console debug:impulse:components</code>
</pre>