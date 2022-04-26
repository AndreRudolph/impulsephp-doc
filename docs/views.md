---
layout: post
title: Views
published: true
---
<h3 class="doc-title">Views</h3>

- [Introduction](#introduction)
- [Load views](#load-views)
- [View inheritance](#view-inheritance)

<h4><a id="introduction">Introduction</a></h4>

The Impulse PHP Framework is based on the awesome Symfony framework and is profiting of its powerful template engine Twig. If you have already been working with Twig you can simply go on with this documentation. In case you haven't, we highly recommend you to have a look at the <a href="https://twig.symfony.com/">Twig documentation</a>.

Views (templates) work quite a bit different than you are used to with Symfony. In a traditional Symfony application a controller is responsible for rendering required views. With the Impulse framework is the other way around as you will see in the controllers documentation.

The following example is a minimal example with just a simple text output.

  <pre class="code-white line-numbers language-twig">
      <code class="language-twig">&lt;impulse&gt;
          &lt;window&gt;Hello world&lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
  
Encapsulating the content in impulse tags is purely optional and the file extension is the same as it is in traditional Symfony applications: twig.html.

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