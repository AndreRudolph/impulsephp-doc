---
layout: post
title: Views
published: true
---
<h1 class="doc-title">Views</h1>

- [Introduction](#introduction)
- [Custom attributes](#custom-attributes)
- [Import views](#import-views)
- [Apply controllers to views](#apply-controllers)

<a name="introduction"></a>

A view (or template) is different to _normal_ templates in symfony. In impulse every tag must represent a component which will be later converted to HTML. Components will be covered in the next documentation page.

The following example is a minimum example with just a simple text output.

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
  <pre class="code-white line-numbers language-twig">
      <code class="language-twig">&lt;impulse&gt;
          &lt;window&gt;
              Hello world
          &lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
</div>
 
A view must start with an opening and must end with a closing impulse tag. The file extension is the same as it is in symfony: twig.html.

As impulse seamlessly integrates twig, even more complex templates can be created.

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
  <pre class="code-white line-numbers language-twig">
      <code class="language-twig">&lt;impulse&gt;
          &lt;window&gt;
          	  &lt;ul&gt;{% raw  %}
          	      {% for i in 0..10 %}
    		      &lt;li&gt;{{ i }}&lt;/li&gt;
			      {% endfor %}
              {% endraw  %}&lt;/ul&gt;
          &lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
</div>

The above example displays a list with ten items counting up from 0 to 9. 

<a name="custom-attributes"></a>
## Custom attributes
Each component can be extended by custom attributes. These attributes may be used to parametrize components.

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
  <pre class="code-white line-numbers language-twig">
      <code class="language-twig">&lt;impulse&gt;
          &lt;window&gt;
              &lt;button&gt;
                  &lt;custom-attributes attr1="val1" attr2="val2" /&gt;
              &lt;/button&gt;
          &lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
</div>

Custom attributes of a component are only accessible on server side. You may access the attributes by directly using the methods **_getCustomAttribute_**, **_getCustomAttributes_** of the component instance within the controller.

<a name="import-views"></a>
## Import views

The framework also provides a possibility to nest views:

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
  <pre class="code-white line-numbers language-twig">
      <code class="language-twig">&lt;impulse&gt;
          &lt;window&gt;
              &lt;import src="helloWorld.imp" /&gt;
          &lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
</div>

<a name="apply-controllers"></a>
## Applying controllers to views
In the next documentation section for controllers you will learn how to connect a view with a controller.
