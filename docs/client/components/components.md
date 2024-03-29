<h3 class="doc-title">Components</h3>

- [Introduction](#introduction)
- [Writing components](#create-components)
  - [States](#states)
  - [Updates](#updates)
- [Component lifecycle](#component-lifecycle)
- [Synchronize with server](#synchronize-with-server)
- [Learn more about components](#advanced_topics)

<h4><a id="introduction">Introduction</a></h4>


Components exist on both server and client side. Each component that is defined on server side is rendered as a client
side component.

The component system is based on React components. If you are not familiar with React. we highly recommend to first get
used to <a href="https://reactjs.org/">React</a>.

<h4><a id="create-components">Writing component</a></h4>

All component classes must either directly or indirectly extend the <span class="code-hint">AbstractReactComponent</span>
class. The properties of the server side component that are shared with the client component are passed in the constructor.

Let's assume we have a Counter client component that receives the initial counter value from its server's counterpart.

For rendering the component into the DOM you can implement the <span class="code-hint">getTemplate</span> method.

<pre class="imp-code code-white language-js code-xl">
<code class="language-js">import { AbstractReactComponent } from '@impulsephp/client-ts';

export class Counter extends AbstractReactComponent
{
    private counter: integer;

    constructor(props)
    {
        super(props);
        this.counter = props.counter;
    }

    getTemplate(attributes)
    {
        return (
            &lt;div {...attributes}&gt;{ this.counter }&lt;/div&gt;
        );
    }
}</code>
</pre>

By adding <span class="code-hint">{...attributes}</span> you ensure that all attributes (id, data attributes, class, etc.)
coming from the parent classes are set correctly.

Thanks to React, you can use <span class="code-hint">Conditional rendering</span> here.

When you also want to allow
child components be rendered, then you must use the <span class="code-hint">this.includeChildren()</span> here.

<pre class="imp-code code-white  language-js">
<code class="language-js">getTemplate(attributes)
{
    return (
        &lt;div {...attributes}&gt;
            { this.getState('counter') }
            { this.includeChildren() }
        &lt;/div&gt;
    );
}</code>
</pre>

<h5><a id="states">States</a></h5>

A common practice is to register properties as state in React. The <span class="code-hint">initializeStates</span> method
will be called automatically in the parent constructor.

<pre class="imp-code code-white  language-js">
<code class="language-js">export class Counter extends AbstractReactComponent
{
    initializeStates(props)
    {
        super.initializeStates(props);

        this.addStates({
            counter: props.counter
        });
    }
}</code>
</pre>

This enables React to re-render the component whenever the message state changes.

<h5><a id="updates">Updates</a></h5>

Components can receive updates from their respective server component. Let's assume the message property will be updated
on server side, a setter must be provided to receive the updated value.

<pre class="imp-code code-white  language-js">
<code class="language-js">export class Message extends AbstractReactComponent 
{
    setMessage(message) 
    {
        this.setState({ message: message });
    }
}

window.Message = Message;</code>
</pre>

Whenever a new message is set, React internally triggers again the rendering process of the component. Thus you don't
need to manually update the Document Object Model.

<h4><a id="component-lifecycle">Component lifecycle</a></h4>

TODO with image

<h4><a id="synchronize-with-server">Synchronize with server</a></h4>

When creating custom or extending existing components, you might need to synchronize client state of a component
(e.g. text input or a selected item) with the server. For this purpose Impulse identifies
<span class="code-hint">dirty</span> components and automatically synchronizing them with the server.

However, you need to register attributes as syncable.

<pre class="imp-code code-white  language-js">
<code class="language-js">private value: string;

constructor(props)
{
    this.addSyncable("propertyName");
}</code>
</pre>