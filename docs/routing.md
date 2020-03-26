<h1 class="doc-title">Routing</h1>

- [Introduction](#introduction)
- [Defining routes](#defining_routes)

<a name="introduction"></a>
## Introduction
Routing is a key concept of a full stack framework and therefore essential. A route defines an entry point (e.g. an URI) of the application and refers to a specific callback or function that handles incoming requests that match this route. 


<a name="defining_routes"></a>
## Defining routes

Defining routes with the Impulse framework is not very different from Symfony's routes definition. The following example shows a route definition.

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
  <pre class="code-white line-numbers language-yaml">
  	<code class="language-yaml">welcome:
      path: welcome
      defaults: { view: app/index.imp }</code>
  </pre>
</div>

Unlike Symfony itself, routes are not bind to any controller. Instead they are bind to a specific view. The view applies to a Controller and not reverse.
