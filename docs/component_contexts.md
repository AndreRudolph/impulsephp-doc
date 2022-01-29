<h3 class="doc-title">Component contexts</h3>

The ComponentClassRegistry is the lookup class in which all discovered components are registered. Remember the following command to list all available components.

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

As you maybe noticed each component is registered at least twice. For example the button component is registered as 'button' and 'impulse:button'. This means you can use both <span class="code-hint">button</span> and <span class="code-hint">impulse:button</span> within your template files.

The lookups for the app itself are placed inside the <span class="code-hint">impulse.yaml</span> config file.

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
  	<code class="language-yaml">impulse:
    client:
        bundles: [ 'ImpulseBundle' ]
        
    components:
    	lookups:
        	- { path: '%kernel.project_dir%/src/UI/Components', namespace: 'App\UI\Components\', label: 'app', priority: 0 }</code>
  </pre>
</div>

Since each bundle (app is also considered as bundle in this context) can also registers it's own components, the following question might raise up: If the app (or any other bundle) provides a custom implementation of a button (same class name as the standard button), how can the Impulse framework distinguish between them? The answer in short is contexts.






The reason for this is that each bundle (the app itself is in this context considers as a bundle) can register its own set of components additionally to the components provided by the Impulse PHP Framework. 


The reason behind this is that the Impulse framework offers you the opportunity to have multiple button components registered.

Consider the following scenario in which you have a custom Button component impelementation in your app project:

App/
	UI/
    	Components/
        	Button.php
            
If there would be no feature like contextual components, every use of <span class="code-hint">button</span> in your templates would use the latest registered Button component - in this case: the app's Button class. But what if you just want to use this button implementation on a few occasions and apart from that the Impulse Button component should be used? For this very reason you can prefix the components with a context:

In the example above in the App is another button component. The component scan discovery overwrites non contextual aliases with the last find. According to this, the following button

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
  <pre class="code-white line-numbers language-markup">
    <code class="imp-code language-markup">&lt;impulse&gt;
        &lt;impulse:button /&gt;
    &lt;/impulse&gt;</code>
  </pre>
</div>
