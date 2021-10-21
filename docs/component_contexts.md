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

As you maybe noticed each component is registered at least twice. For example the button component is registered as 'button' and 'impulse:button'. The reason behind this is that the Impulse framework offers you the opportunity to have multiple button components registered.

Consider the following scenario:
App/
	UI/
    	Components/
        	Button.php

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
        &lt;button /&gt;
    &lt;/impulse&gt;</code>
    </pre>
</div>

will be bound to the app button component class. However, you can also explicitly use the button implementation that is provided by the Impulse framework with a contextual prefix.

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