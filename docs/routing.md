<h1 class="doc-title">Routing</h1>

- [Introduction](#introduction)
- [Defining routes](#defining_routes)
- [Entry points](#entry-points)

<a name="introduction"></a>
## Introduction
Routing is a key mechanism of a full stack framework and therefore essential. A route defines an entry point (e.g. an URI) of the application and refers to a specific callback or function that handles incoming requests that match this route. 

For a general overview of the routing concept, you can read the symfony's <a href="https://symfony.com/doc/current/routing.html" target="_blank">documentation</a>.


<a name="defining_routes"></a>
## Defining routes

Route definitions in the Impulse framework are similar to symfonys native route definitions and uses the default parameters for required informations. The following example shows an extended route definition.

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

The view parameter is necessary and will be explained more in depth in the following Entry points chapter. 

<a name="entry-points" />
## Entry points

As mentioned above, routes represent entry points. By using impulse, a route consists of two entry points. First one is the **_FrontController_** (App\Controller\ImpulseController) which works like a normal Symfony controller but routes every incoming, impulse related, request to the impulse engine. The engine then renders the given view template which represents the entry point of the impulse application itself.

The complete guide to views can be found in the following <span class="link" data-target-menu-item="views">Views</span> chapter.
