---
layout: post
title: Start of Documentation
published: true
---
<h1 class="doc-title">Introduction</h1>

- [Introduction](#introduction)
- [Custom attributes](#custom-attributes)
- [Import views](#import-views)
- [Apply controllers to views](#apply-controllers)

<a name="introduction"></a>

Views in Impulse are almost as simple as HTML but very powerful. A view encapsulates a set of components which will be later converted by the Impulse js engine to HTML. The following example is a minimal example with just a simple text output.

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
            <div class="button red" />
          	<div class="button yellow" />
          	<div class="button green" />
        </div>
    </div>
  </div>
  <pre class="code-white line-numbers language-markup">
      <code class="language-markup">&lt;impulse&gt;
          &lt;window&gt;
              Hello world
          &lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
</div>
 
A view must start with an opening impulse tag and must be closed with a closing impulse tag.

<a name="custom-attributes"></a>
## Custom attributes
Each component within a view can be extended by custom attributes. These attributes may be used to customize special components for e.g. specific processing with the component.

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
            <div class="button red" />
          	<div class="button yellow" />
          	<div class="button green" />
        </div>
    </div>
  </div>
  <pre class="code-white line-numbers language-markup">
      <code class="language-markup">&lt;impulse&gt;
          &lt;window&gt;
              &lt;button&gt;
                  &lt;custom-attributes attr1="val1" attr2="val2" /&gt;
              &lt;/button&gt;
          &lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
</div>

Custom attributes of a component are only accessible on server side. You may access them to handle components with custom attributes in that way that fit your needs.

<a name="import-views"></a>
## Import views

The framework also provides a possibility to nest views:

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
            <div class="button red" />
          	<div class="button yellow" />
          	<div class="button green" />
        </div>
    </div>
  </div>
  <pre class="code-white line-numbers language-markup">
      <code class="language-markup">&lt;impulse&gt;
          &lt;window&gt;
              &lt;import src="helloWorld.imp" /&gt;
          &lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
</div>

<a name="apply-controllers"></a>
## Applying controllers to views
In the next documentation section for controllers you will learn how to connect a view with a controller.
