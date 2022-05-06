---
layout: post
title: Views
published: true
---
<h3 class="doc-title">Views</h3>

- [Introduction](#introduction)
- [Load views](#load-views)
- [View inheritance](#view-inheritance)
- [Scopes](#scopes)

<h4><a id="introduction">Introduction</a></h4>

The Impulse PHP Framework is based on the awesome Symfony framework and is profiting of its powerful template engine Twig. If you have already been working with Twig you can simply go on with this documentation. In case you haven't, we highly recommend you to have a look at the <a href="https://twig.symfony.com/" target="_blank">Twig documentation</a>.

Views (templates) work quite a bit different than you are used to with Symfony. In a traditional Symfony application a controller is responsible for rendering required views. With the Impulse framework it is the other way around as you will see in the controllers documentation.

The following example is a minimal example with just a simple text output.

  <pre class="imp-code code-white line-numbers language-twig">
      <code class="language-twig">&lt;impulse&gt;
          &lt;window&gt;Hello world&lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
  
The impulse tags are optional but are used for all examples here for sake of completeness. However, 
every template must not have more than one root tag. The file extension is the same as it is in traditional Symfony applications: twig.html.

<h5><a id="load-views">Load views</a></h5>

It is a common good practice (separation of concerns) to separate views into smaller pieces to become reusable and more descriptively. Hence it is possible to load views directly into a certain place in another view.

  <pre class="code-white line-numbers language-twig">
      <code class="language-twig">&lt;impulse&gt;
          &lt;window&gt;
              &lt;import src="helloWorld.html.twig" /&gt;
          &lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
  
  <pre class="code-white line-numbers language-twig">
      <code class="language-twig">&lt;impulse&gt;
          &lt;window&gt;
              &lt;window&gt;Hello world&lt;/window&gt;
          &lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>

<h5><a id="view-inheritance">View inheritance</a></h5>

Yet another great feature of Twig is called template inheritance. By having that, you can define common templates with so called blocks that can be overriden by a derived template.

<h5><a id="scopes">Scopes</a></h5>

Though scopes are not relevant for implementing views it is good to at least know the basics of this concept.

Every view will have it's own scope in place. A scope is a technical and organizational concept for a scoped set of components. Each scope has an unique identifier (scope id) that identifies the scope and all of its descendant components. Below you will find a picture with an example view structure and its scope ids.

<img src="build/static/scopes.png" />

As you can see, each view has a scope id and every root component of a view serves as their respective scope owner. Thus, every scope has a root component and references to the parent and children scopes. These references are managed automatically by the framework.
