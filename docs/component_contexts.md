<h3 class="doc-title">Component contexts</h3>

- [Introduction](#introduction)
- [Contextual use](#contextual-use)
- [Customize registration order](#customize-registration-order)

<h4><a id="introduction">Introduction</a></h4>

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

<h4><a id="contextual-use">Contextual use</a></h4>

By default all component names without the context (e.g. impulse:) are automatically mapped to the components that are provided by the framework (impulse context). 

Since each bundle (app is also considered as bundle in this context) can registers it's own components, the following question might raise up: If the app (or any other bundle) provides a custom implementation of a button (same class name as the standard button), how can the Impulse framework distinguish between the app's button and the impulse's button? The answer in short is contexts.

Let's consider you have your own implementation of a button and the classes is placed in App/UI/Components. You can use buttons now in three different ways.

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
  	<code class="language-markup">&lt;impulse&gt;
    	&lt;button /&gt; &lt;!-- uses App\UI\Components\Button class --&gt;
        &lt;app:button /&gt; &lt;!-- uses App\UI\Components\Button class --&gt;
        &lt;impulse:button /&gt; &lt;!-- uses Impulse\ImpulseBundle\UI\Components\Button class --&gt;
    &lt;/impulse&gt;</code>
  </pre>
</div>

So whenever you use <span class="code-hint">button</span> then the app's button is used. This default behavior is used when you globally want to replace default components with your own components or bundled components.

<h4><a id="customize-registration-order">Customize registration order</a></h4>

There might be cases where you do not want to have any automatic overwrites and instead want to keep the components without contexts using the Impulse components rather than your app components. To achieve this, you can change the tag priority in the services.yaml file.

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
  <pre class="code-white imp-code line-numbers language-yaml">
  	<code class="language-yaml">services:
    	App\UI\Components\:
        	resource: '../src/UI/Components'
            tags:
            	- { name: 'impulse.components', priority: 500 }</code>
  </pre>
</div>

Thanks to Symfony, you can switch the behavior so that the usage of the tag <span class="code-hint">button</span> uses the impulse button rather than the app button. This is useful if you just want to replace the button only for some occurences and not all of them!