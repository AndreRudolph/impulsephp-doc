<h3 class="doc-title">Components</h3>

- [Introduction](#introduction)
- [Create components](#create-components)
- [Updates from server](#updates-from-server)
- [Component lifecycle](#component-lifecycle)
- [Event listener](#event-listener)
- [Learn more about components](#advanced_topics)

<h4><a id="introduction">Introduction</a></h4>

Components exist on both server and client side. Each component that is defined on server side is rendered as a client
side component. 

The component system is based on React components. If you are not familiar with React. we highly recommend to first get 
used to <a href="https://reactjs.org/">React</a>.

<h5><a id="create-components">Create custom component</a></h5>

All component classes must either directly or indirectly extend the <span class="code-hint">AbstractReactComponent</span>
class. The constructor takes the properties from the server side component as argument.

<pre class="imp-code code-white line-numbers language-js">
	<code class="language-js">
import { AbstractReactComponent } from '@impulsephp/client-ts';

export class Message extends AbstractReactComponent
{
    private message: string;
    constructor(props)
    {
        super(props);
        this.message = props.message;
    }
}</code>
</pre>

A common practice is to register properties as state in React. The <span class="code-hint">initializeStates</span> method
will be called automatically in the parent constructor.

<pre class="imp-code code-white line-numbers language-js">
	<code class="language-js">
export class Message extends AbstractReactComponent
{
    initializeStates(props)
    {
        super.initializeStates(props);
        this.addStates({
            message: props.message
        });
    }
}</code>
</pre>

This enables React to re-render the component whenever the message state changes.

<h6>Rendering</h6>

For most use cases you can implement the <span class="code-hint">getTemplate</span> method to render the component.

<pre class="imp-code code-white line-numbers language-js">
	<code class="language-js">
export class Message extends AbstractReactComponent
{
    getTemplate(attributes)
    {
        return (
            &lt;div {...attributes}&gt;{ this.getState('message') }&lt;/div&gt;
        );
    }
}</code>
</pre>

Thanks to React, you can use <span class="code-hint">Conditional rendering</span> here.

<h5><a id="updates-from-server">Updates from server</a></h5>

Components can receive updates from their respective server component. Let's assume the message property will be updated
on server side, a setter must be provided to receive the updated value.

<pre class="imp-code code-white line-numbers language-js">
	<code class="language-js">
export class Message extends AbstractReactComponent 
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