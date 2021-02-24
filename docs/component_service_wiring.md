<h1 class="doc-title">Component service wiring</h1>

- [Introduction](#introduction)
- [Wire services](#wire-services)

<a id="#introduction"><h4>Introduction</h4></a>

The concept of components is that a component can literally be anything, a simple div or an textbox or can be even more complex like a combobox which provided items stored in a database.
However, whenever your component requires a service your component must implement the **_WiringAware_** interface.

Marked with that interface, the Impulse framework will delegate the component object construction to the symfony dependency injection container. Thus you can inject services as usual via constructor or setter for optional dependencies.

<div class="code-header">
	<div class="container-fluid">
		<div class="row">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
    </div>
</div>
<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php
	namespace App\UI\Components;
    use Impulse\ImpulseBundle\UI\Components\AbstractComponent;
    use Impulse\ImpulseBundle\Components\WiringAware;
    use App\Services\MyService;

    class SpecialComponent extends AbstractComponent implements WiringAware
    {
        public function __construct(MyService $service) 
        {
            // initialization
        }
	}</code>
</pre>


