<h3 class="doc-title">Validation & states</h3>

- [Introduction](#introduction)
- [Component state](#component-state)
- [Validation with validator](#validation)
    - [Validator and rules](#validator-and-rules)
    - [Custom handling](#custom-handling)
    - [Available rules](#available-rules)

<h4><a id="introduction">Introduction</a></h4>
Validation is a key requirement of any application when it comes to user's input. The Impulse framework offers some good mechanics to have a comfortable and yet flexible way of validating forms. 

<h4><a id="component-state">Component state</a></h4>

Every component that extends from <span class="code-hint">AbstractComponent</span> provides a built-in solution for a component state. In detail, the state comprises of three different attributes: state, state message and state type.

The state itself can be any name you need to have. By default there are four different states. 

<ul class="contentList">
  <li><span class="code-hint">true</span> indicates that the component might have a state in the future and enables the state handling. The DOMs structure will be prepared for having a state and a state message but nothing is yet visualized.</li> 
  <li><span class="code-hint">ok</span> indicates that everything is fine</li>
  <li><span class="code-hint">warning</span> indicates that the input in general will be accepted but the user will be warned about potential side effects</li>
  <li><span class="code-hint">error</span> indicates an error that prevents further processing</li>
</ul>

The state can be set by calling the <span class="code-hint">setState</span> method of the component.

Secondly the message that shall appear to the user can be set by calling the <span class="code-hint">setState</span> method.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
namespace App\Controller;

use Impulse\ImpulseBundle\Controller\AbstractController;
use Impulse\ImpulseBundle\UI\Components\Textbox;
use Impulse\ImpulseBundle\UI\Components\ComponentInterface;
use Impulse\ImpulseBundle\Execution\Events\Event;


class MyController extends AbstractController
{
    private ?Textbox $tb = null;
    
    public function afterCreate(Event $event): void
    {
        parent::afterCreate($event);
        $this->tb->setState(ComponentInterface::STATE_ERROR);
        $this->tb->setStateMessage('Something is wrong with the input.');
    }
}</code>
</pre>

There is another property called <span class="code-hint">stateType</span> that can be set by calling <span class="code-hint">stateType</span>. The type declares the way of visualization. The default value is "default."

All components extending the InputComponent have a default implementation of state handling and visualization. With default value the input element of the component is extended with a css class and the message appears on bottom of it. By changing the type to "tooltip", the message will instead appear as a tooltip.

<h4><a id="component-state">Validation with validator</a></h4>

While you can use validation without the previous described state feature, it is highly recommended to use both of them together. This chapter will focus on using both of them but you are free to implement your own validation handling. 

Basically you have two different approaches for doing validations. One is the usage of the provided validator and its rules or by validtating inputs by hand. The validor uses the awesome <a href="https://github.com/floriankraemer/validation" target="_blank">Somnambulist Validation library</a>.

<h5><a id="validator-and-rules">Validator and rules</a></h5>

For using the validator, the components that need to be validated must have set the validation rules. The validation rules are an associative array that defines the properties to validate as keys and the rules as their respective values:

<pre class="imp-code code-white language-php">
<code class="language-php">$this->tb->setValidationRules(['value' => 'required|min:20']);</code>
</pre>

You may also set the rules within a template file. Since the arguments of the attributes are interpreted as a string, you have to use a different notation so that the <span class="code-hint">setValidationRules</span> setter can explode it to an array.

<pre class="imp-code code-white language-twig">
<code class="language-twig">&lt;textbox id="tb" validationRules="value@required|min:20" /&gt;</code>
</pre>

The validator can then be retrieved from the <span class="code-hint">AbstractController</span> with the <span class="code-hint">getValidator</span> method. You can simply call <span class="code-hint">validate</span> and pass a variadric list of components or callables (they will be covered later on).

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
namespace App\Controller;

use Impulse\ImpulseBundle\Controller\AbstractController;
use Impulse\ImpulseBundle\UI\Components\Textbox;
use Impulse\ImpulseBundle\Execution\Events\Event;

class MyController extends AbstractController
{
    private ?Textbox $tb = null;
    
    public function afterCreate(Event $event): void
    {
        parent::afterCreate($event);
        $result = $this->getValidator()->validate($this->tb);
        
        if ($result->passes()) {
            // everything is fine, do further processing 
        }
    }
}</code>
</pre>

The return value of the validation is a <span class="code-hint">Impulse\ImpulseBundle\Components\Validation\ValidationResult</span> object and offers convenient methods for further validation handling. However, in the example above the validator has automatic handling enabled. This means, that the validation result will be flattened and for each failed component the state will be set to error state and the validation message will be set to the state message.

The result object offers different methods for working with the validation results:

<ul class="contentList">
  <li><span class="code-hint">passes()</span> returns true if the validation was successful and false if not</li>
  <li><span class="code-hint">fails()</span> returns true if the validation failed and true if validation passed</li>
  <li><span class="code-hint">getFailedValidationsCount()</span> returns the number of failed validations</li>
  <li><span class="code-hint">getResult()</span> returns a map with all failed validation results for all components. You will most likely need this if you need multiple validation messages per input.</li>
  <li><span class="code-hint">getFlatResult()</span> returns a map with exactly one failed validation per component. You will most likely need this if you need only one validation message per input.</li>
</ul>

<h5><a id="custom-handling">Custom handling</a></h5>

In the previous section you learned how to use the validator to validate against components with no manual handling of the error visualization. However, you might need to take full control of the validation result handling. For this purpose you can call <span class="code-hint">disableAutoHandling</span>.

<pre class="code-white language-php">
<code class="imp-code language-php">$result = $this->getValidator()->disableAutoHandling()->validate($this->tb);</code>
</pre>

With the methods of the result object you will have maximum flexibility to build your own way of validation handling and visualization.

<h5><a id="available-rules">Available rules</a></h5>
