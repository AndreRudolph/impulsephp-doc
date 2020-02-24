# Routing

- [Defining routes](#defining_routes)

<a name="defining_routes"></a>
## Defining routes

Defining routes with the Impulse framework is not very different from Symfony's routes definition. The following example shows a route definition.

<pre class="line-numbers language-yaml">
<code class="language-yaml">welcome:
    path: welcome
    defaults: { view: app/index.imp }</code>
</pre>
