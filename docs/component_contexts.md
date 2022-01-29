<h3 class="doc-title">Component contexts</h3>

All available components are automatically registered within the class <span class="code-hint">ComponentClassRegistry</span>. Remember the following command to list all available components.

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
  <pre class="code-white imp-code line-numbers language-shell">
  	<code class="language-bash">php bin/console debug:impulse:components</code>
  </pre>
</div>

As you maybe noticed each component is registered at least twice. For example the button component is registered as 'button' and 'impulse:button'. This means you can use both <span class="code-hint">button</span> and <span class="code-hint">impulse:button</span> within your template files and in both cases the same button component is used.

By default all component names without the context (e.g. impulse:) are automatically mapped to the components that are provided by the framework (impulse context). 

Since each bundle (app is also considered as bundle in this context) can registers it's own components, the following question might raise up: If the app (or any other bundle) provides a custom implementation of a button (same class name as the standard button), how can the Impulse framework distinguish between the app's button and the impulse's button? The answer in short is contexts.

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
  <pre class="code-white imp-code line-numbers language-markup">
  	<code class="language-markup">fooo</code>
  </pre>
</div>
