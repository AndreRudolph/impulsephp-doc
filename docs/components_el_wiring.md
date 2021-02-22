<h1 class="doc-title">Components Expression Language based Wiring</h1>

- [Introduction](#introduction)
- [Available functions](#available-functions)
	- [instanceOf()](#instanceOf)
    - [component(attribute)](#componentAttribute)
    - [id()](#id)
    - [uid()](#uid)

<a href="#introduction">Introduction</a>

Sometimes it is necessary to find direct descendant components by a condition within the same tree of components. This is the use case the annotation  **_Wire_** was designed for.

You can define the component(s) that will be wired into an attribute of the class by using symfonys awesome expression language. Impulse extends the expression language by adding four different functions to retrieve components based on their state and class instance. The functions are explained in the sections below.

<a href="#instanceOf">instanceOf()</a>

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
