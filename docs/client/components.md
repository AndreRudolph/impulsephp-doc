<h3 class="doc-title">Components</h3>

- [Introduction](#introduction)
- [Writing components](#create-components)
  - [States](#states)
  - [Rendering](#rendering)
  - [Updates](#updates)
- [Component lifecycle](#component-lifecycle)
- [Event listener](#event-listener)
  - [Emit events](#emit-events)
  - [Primitive Listener](#primitive-listener)
  - [Add listener](#adding-listener)
  - [Remove listener](#removing-listener)
- [Synchronize with server](#synchronize-with-server)
- [Learn more about components](#advanced_topics)

<h4><a id="introduction">Introduction</a></h4>

Components exist on both server and client side. Each component that is defined on server side is rendered as a client
side component. 

The component system is based on React components. If you are not familiar with React. we highly recommend to first get 
used to <a href="https://reactjs.org/">React</a>.

<h4><a id="create-components">Writing component</a></h4>

All component classes must either directly or indirectly extend the <span class="code-hint">AbstractReactComponent</span>
class. The constructor takes the properties from the server side component as argument.

<pre class="imp-code code-white line-numbers language-js">
<code class="language-js">import { AbstractReactComponent } from '@impulsephp/client-ts';

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
<code class="language-js">export class Message extends AbstractReactComponent
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
<code class="language-js">export class Message extends AbstractReactComponent
{
    getTemplate(attributes)
    {
        return (
            &lt;div {...attributes}&gt;{ this.getState('message') }&lt;/div&gt;
        );
    }
}</code>
</pre>

Thanks to React, you can use <span class="code-hint">Conditional rendering</span> here. When you also want to allow
child components be rendered, then you must use the <span class="code-hint">this.includeChildren()</span> here.

<pre class="imp-code code-white line-numbers language-js">
<code class="language-js">export class Message extends AbstractReactComponent
{
    getTemplate(attributes)
    {
        return (
            &lt;div {...attributes}&gt;
                { this.getState('message') }
                { this.includeChildren() }
            &lt;/div&gt;
        );
    }
}</code>
</pre>

<h6><a id="updates">Updates</a></h6>

Components can receive updates from their respective server component. Let's assume the message property will be updated
on server side, a setter must be provided to receive the updated value.

<pre class="imp-code code-white line-numbers language-js">
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

<h4><a id="event-listener">Event listener</a></h4>

When it comes to components you will sooner or later need to implement your custom event listeners. Impulse uses the
same attribute notion React uses for registering listener, e.g. <span class="code-hint">onClick={doSomething()}</span>. 
The problem with this notion is, that you can't register / unregister separate event listeners dynamically for the same 
event. 

Thus, each Impulse client component internally stores a priority event listener callback map which enables
you to register / unregister your custom event listeners at runtime without touching any existing listener for the same
event. Checkout the next chapter for a more code based explanation.

<h5><a id="emit-events">Emit events</a></h5>

Event listener should be emitted by calling the emit method whenever you want an event to be processed.

<pre class="imp-code code-white line-numbers language-js">
<code class="language-js">export class Message extends AbstractReactComponent 
{
    getTemplate(attributes) {
        return (
            &lt;button 
                {...attributes} 
                click={(event) => this.emitEvent(event, 'click')} 
            &gt;
                Click me
            &lt;/button&gt;
        );
    }
}

window.Message = Message;</code>
</pre>

This, internally, will run through every registered click listener that is attached to the click event. In the next 
chapter you'll learn how to register listener.

<h6><a id="adding-listener">Add listener</a></h6>
To register your event listener you can implement the registerEventListener method.

<pre class="imp-code code-white line-numbers language-js">
<code class="language-js">export class Message extends AbstractReactComponent 
{
    registerEventListener()
    {
        super.registerEventListener();

        this.addEventListener((event) => {
            console.log('I am listener 1.');
        }, 'click', 400);

        this.addEventListener((event) => {
            console.log('I am listener 2.');
        }, 'click', 500);
    }
}

window.Message = Message;</code>
</pre>

As you can see the code registers two click listener with different priorities. The higher the priority, the
earlier the callback will be executed. This means, first the second listener code will be executed and afterwards the
first one.

<h6><a id="Removing-listener">Remove listener</a></h6>

TODO

<h4><a id="synchronize-with-server">Synchronize with server</a></h4>

When creating custom or extending existing components, you might need to synchronize client state of a component 
(e.g. text input or a selected item) with the server. For this purpose Impulse identifies 
<span class="code-hint">dirty</span> components and automatically synchronizing them with the server.

However, you need to register attributes as syncable.

<pre class="imp-code code-white line-numbers language-js">
<code class="language-js">constructor(props)
{
    
}</code>
</pre>