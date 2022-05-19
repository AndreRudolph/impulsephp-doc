<h3 class="doc-title">Validation & states</h3>

- [Introduction](#introduction)
- [Component state](#component-state)
- [Validation](#validation)
    - [Validator and rules](#validator-and-rules)
    - [Manual validation](#manual-validation)

<h4><a id="introduction">Introduction</a></h4>
Validation is a key requirement of any application when it comes to user's input. The Impulse framework offers some good mechanics to have a comfortable and yet flexible way of validating forms. 

<h4><a id="component-state">Component state</a></h4>

Every component that extends from <span class="code-hint">AbstractComponent</span> provides a built-in solution for a component state. In detail, the state comprises of three different attributes.

The state itself can be any name you need to have. By default there are four different states. 

- <span class="code-hint">true</span> indicates that the component might have a state in the future and enables the state handling. The DOMs structure will be prepared for having a state and a state message but nothing is yet visualized. 
- <span class="code-hint">ok</span> indicates that everything is fine
- <span class="code-hint">warning</span> indicates that the input in general will be accepted but the user will be warned about potential side effects
- <span class="code-hint">error</span> indicates an error that prevents further processing

The state can be set by calling the <span class="code-hint">setState</span> method of the component.

Secondly the message that shall appear to the user can be set by calling the <span class="code-hint">setState</span> method.

<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php
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

<h4><a id="component-state">Validation</a></h4>

While you can use validation without the previous described state feature, it is highly recommended to use both of them together. This chapter will focus on using both of them but you are free to implement your own validation handling. 

Basically you have two different approaches for doing validations. One is the usage of the provided validator and its rules or by validtating inputs by hand. The validor uses the awesome <a href="https://github.com/floriankraemer/validation" target="_blank">Somnambulist Validation library</a>.

<h5><a id="validator-and-rules">Validator and rules</a></h5>

For using the validator, the components that need to be validated must have set the validation rules. The validation rules are an associative array that defines the properties to validate as keys and the rules as their respective values:

<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php
    
    $this->tb->setValidationRules(['value' => 'required|min:20']);
    </code>
</pre>

You may also set the rules within a template file. Since the arguments of the attributes are interpreted as a string, you have to use a different notation so that the <span class="code-hint">setValidationRules</span> setter can explode it to an array.

  <pre class="code-white line-numbers language-twig">
      <code class="language-twig">&lt;impulse&gt;
          &lt;window&gt;
              &lt;textbox id="tb" validationRules="value@required|min:20" /&gt;
          &lt;/window&gt;
      &lt;/impulse&gt;</code>
  </pre>
