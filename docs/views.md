---
layout: post
title: Views
published: true
---
<h3 class="doc-title">Views</h3>

- [Introduction](#introduction)
- [Twig integration](#twig-integration)
- [Custom attributes](#custom-attributes)
- [Load views](#load-views)

<h4><a id="introduction">Introduction</a></h4>

A view (or template) is different to _normal_ templates in symfony. In impulse every tag must represent a component (except the impulse tag and custom attributes) which will be later converted to HTML. Components will be covered in a later documentation page.

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

<h5><a id="twig-integration">Twig integration</a></h5>

One benefit of impulse is it seamless integration of twig since it is used behind the scenes.

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

<h5><a id="custom-attributes">Custom attributes</a></h5>

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

Custom attributes of a component are only accessible on server side. You may access the attributes by directly using the methods **_getCustomAttribute(attributeName)_** or **_getCustomAttributes_** of the component instance within the controller.

<h5><a id="load-views">Load views</a></h5>

It is a common good practice (separation of concerns) to separate views into smaller pieces to become reusable and more descriptively. Hence it is possible to load (import) views directly into a view.

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

With this separation your application becomes more extensible and maintainable when to it comes to controllers in the next documentation page. 
