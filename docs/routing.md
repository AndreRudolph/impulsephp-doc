<h1 class="doc-title">Routing</h1>

- [Introduction](#introduction)
- [Defining routes](#defining_routes)

<a name="introduction"></a>
## Introduction
Routing is a key mechanism of a full stack framework and therefore essential. A route defines an entry point (e.g. an URI) of the application and refers to a specific callback or function that handles incoming requests that match this route. 

For a general overview of the routing concept, you can read the symfony's <a href="https://symfony.com/doc/current/routing.html" target="_blank">documentation</a>.


<a name="defining_routes"></a>
## Defining routes

Route definitions in the Impulse framework are almost similar to symfonys native route definitions. The following example shows an extended route definition.

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
  	<code class="language-yaml">index:
      path: index
      controller: App\Controller\ImpulseController::index
      defaults: { view: app/index.html.twig }</code>
  </pre>
</div>

The example above looks very much familiar to symfonys route definition by one exception. In the Impulse PHP framework, a route can be further configured with a view that will be rendered. The controller 'App\Controller\ImpulseController' is designed as a front controller and represents the entry point of the framework. 


