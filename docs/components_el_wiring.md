<h1 class="doc-title">Components Expression Language based Wiring</h1>

- [Introduction](#introduction)

<a href="#introduction">Introduction</a>

The more complex a component get the more functionalities are necessary. Whenever you face the issue of wiring components from the same component tree, you can do this easily via the **_Wire_** annotation. 

You can define the component(s) that will be wired by using symfonys awesome expression language. Impulse extends the expression language by adding four different functions to retrieve components based on their state and class instance.

- instanceOf
- id()
- uid()
- component(attribute)

The RadioGroup component uses the expression language to wire the direct descendant radio components by using the instanceOf() expression language function provied by Impulse. 

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
	<code class="imp-code language-markup"><?php
namespace Impulse\ImpulseBundle\UI\Components;
use Impulse\ImpulseBundle\Annotations\Wire;
use Impulse\ImpulseBundle\Components\AfterCreateChilds;
use Impulse\ImpulseBundle\Events\Events;
use Tightenco\Collect\Support\Collection;

class RadioGroup extends AbstractComponent implements AfterCreateChilds
{
	#[Wire('instanceOf("Impulse\\\\ImpulseBundle\\\\UI\\\\Components\\\\Radio")')]
    private ?Collection $radios;
}</code>
</pre>
