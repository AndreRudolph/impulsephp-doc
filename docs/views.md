---
layout: post
title: Views
published: true
---
<h1 class="doc-title">Views</h1>

- [Introduction](#introduction)
- [Custom attributes](#custom-attributes)
- [Nesting views](#nesting-views)
- [Apply controllers to views](#apply-controllers)

<a name="introduction"></a>

Views in Impulse are simple to understand but very powerful. A view encapsulates a set of components which will be later converted to HTML in the browser. The following example is a minimal example with just a simple text output.

<pre class="code-white line-numbers language-markup">
	<code class="language-markup">&lt;impulse&gt;
    	&lt;window&gt;
        	Hello world
        &lt;/window&gt;
    &lt;/impulse&gt;</code>
</pre>

A view must start with an opening impulse tag and must be closed with a closing impulse tag.

<a name="custom-attributes"></a>
## Custom attributes
Each component within a view can be extended by custom attributes. These attributes may be used to customize special components for e.g. specific processing with the component.

<pre class="code-white line-numbers language-markup">
	<code class="language-markup">&lt;impulse&gt;
    	&lt;window&gt;
        	&lt;button&gt;
            	&lt;custom-attributes attr1="val1" attr2="val2" /&gt;
            &lt;/button&gt;
        &lt;/window&gt;
    &lt;/impulse&gt;</code>
</pre>

Custom attributes of a component are only accessible on server side. You may access them to handle components with custom attributes in that way that fit your needs.

<a name="nesting-views"></a>
## Nesting views

The framework also provides a possibility to nest views:

<pre class="code-white line-numbers language-markup">
	<code class="language-markup">&lt;impulse&gt;
    	&lt;window&gt;
            &lt;import src="helloWorld.imp" /&gt;
        &lt;/window&gt;
    &lt;/impulse&gt;</code>
</pre>

<a name="apply-controllers"></a>
## Applying controllers to views
In the next documentation section for controllers you will learn how to connect a view with a controller.
