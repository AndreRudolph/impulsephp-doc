<h3 class="doc-title">Attribute initializer</h3>

- [Introduction](#introduction)
- [Create initializer](#create-initializer)
- [Prioritizing](#prioritizing)

<h4><a id="introduction">Introduction</a></h4>
Components internal state is handled automatically in most cases by the Impulse PHP framework. However, components that are needed for processing the current request are reactivated and its state will be restored. You might face the isssue that, in some cases, you need a custom initialization of a specific component attribute. This is where attribute initializer are used for.

<h4><a id="create-listener">Create initializer</a></h4>
An attribute initializer must implement the <span class="code-hint">Impulse\ImpulseBundle\Components\Initializer\Attributes\AttributeInitializer</span> interface. he <span class="code-hint">supports</span> method determins whether this initializer should handle a specific attribute & component combination or not. The <span class="code-hint">apply</span> method applies the initialization of this attribute for the component with the given value.

<pre>
	<code class="imp-code language-php"><?php

	use Impulse\ImpulseBundle\Components\Initializer\Attributes\AttributeInitializer;
    use Impulse\ImpulseBundle\UI\Components\ComponentInterface;

	class MyAttributeInitializer implements AttributeInitializer
	{
        public function supports(ComponentInterface $component, string $attribute, mixed $value): bool
        {
            // check if component and attribute are supported
        }

        public function apply(ComponentInterface $component, string $attribute, mixed $value): void
        {
            // apply the initialization to the component
        }
	}</code>
</pre>

The initializer now needs to be considered by using the <span class="code-hint">impulse.components.attribute_initializer</span> service tag.

<pre class="code-white language-yaml">
	<code class="imp-code language-yaml">services:
    
    App\UI\Components\Initializer\Attributes\MyAttributeInitializer:
        class: App\UI\Components\Initializer\Attributes\MyAttributeInitializer
        tags:
            - { name: 'impulse.components.attribute_initializer', priority: 200 }</code>
</pre>

<h4><a id="prioritizing">Prioritizing</a></h4>
Every attribute initializer has its own priority defined and they are requested in order with higher priority first. However, when you want to priorizize one attribute above or below another, then you must change the priority accordingly. Suppose you want your custom attribute initializer at the very first, you can chose a higher priority.

<pre class="code-white language-yaml">
	<code class="imp-code language-yaml">services:
    
    App\UI\Components\Initializer\Attributes\MyAttributeInitializer:
        class: App\UI\Components\Initializer\Attributes\MyAttributeInitializer
        tags:
            - { name: 'impulse.components.attribute_initializer', priority: 2000 }</code>
</pre>