<h1 class="doc-title">Routing</h1>

- [Introduction](#introduction)
- [Defining routes](#defining_routes)

<a name="introduction"></a>
## Introduction
Routing is a key concept of a full stack framework and therefore essential. A route defines an entry point (e.g. an URI) of the application and refers to a specific callback or function that handles incoming requests that match this route. 

For a general overview of the routing concept, you can read the symfony's <a href="https://symfony.com/doc/current/routing.html" target="_blank">documentation</a>.


<a name="defining_routes"></a>
## Defining routes

Route definitions in the Impulse framework are almost similar to symfonys native route definitions. Consider the following example.

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

Unlike Symfony itself, routes are not bind to any controller. Instead they are bind to a specific view. You can read more about views in the next documentation article.
