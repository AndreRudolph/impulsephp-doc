<h1 class="doc-title">Routing</h1>

- [Defining routes](#defining_routes)

<a name="defining_routes"></a>
## Defining routes

Defining routes with the Impulse framework is not very different from Symfony's routes definition. The following example shows a route definition.

<pre class="code-white line-numbers language-yaml">
<code class="language-yaml">welcome:
    path: welcome
    defaults: { view: app/index.imp }</code>
</pre>

Unlike Symfony itself, routes are not bind to any controller. Instead they are bind to a specific view. The view applies to a Controller and not reverse.
