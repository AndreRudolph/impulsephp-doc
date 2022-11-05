<h1 class="doc-title">Component service wiring</h1>

- [Introduction](#introduction)
- [Wire services](#wire-services)

<h4><a id="#introduction">Introduction</a></h4>

The concept of components is that a component can literally be anything, a simple div or a textbox or can be even more complex like a combobox which provides items stored in a database.
However, whenever your component requires a service your component must implement the **_WiringAware_** interface.

<h4><a id="#wire-services">Wire services</a></h4>

Marked with that interface, the Impulse framework will delegate the component object construction to the symfony dependency injection container. Thus you can inject services as usual via constructor for required or setter for optional dependencies.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
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
