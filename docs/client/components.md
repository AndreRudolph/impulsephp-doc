<h3 class="doc-title">Components</h3>

- [Introduction](#introduction)
- [Create components](#create-components)
- [Component lifecycle](#component-lifecycle)
- [Updates from server](#updates-from-server)
- [Attribute renderer](#attribute-renderer)
- [Event listener](#event-listener)
- [Learn more about components](#advanced_topics)

<h4><a id="introduction">Introduction</a></h4>

This article covers the basics of client implementation of components. In the "Learn about more components" section you will find further advanced topics related to components.

Note that everything related to client needs to have a complete working npm environment as described in the installation manual.

<h5><a id="create-components">Create custom component</a></h5>

<h6>Using getTemplate method</h6>

Creating the client side of components is quite easy. Let's take again that javascript example (Message component) from the server side documentation of components. It's only purpose is to wrap a text inside a simple div container.

<pre class="imp-code code-white line-numbers language-js">
	<code class="language-js">import AbstractComponent from '../../../../vendor/impulsephp/impulsebundle/src/Resources/assets/js/impulse-bundle/Impulse/UI/Components/AbstractComponent';
    
    export default class Message extends AbstractComponent {
        constructor() {
            super();
            this.message = '';
        }

        setMessage(message) {
            this.message = message;
        }

        getTemplate() {
            return '<div><%= component.message %></div>';
        }
    }
    
    window.Message = Message;</code>
</pre>

For very simple components the <span class="code-hint">getTemplate</span> method was introduced. It is used by the base <span class="code-hint">create</span> method implementation of AbstractComponent class that will be covered later in this article.

However, it returns a string based template that under the hood makes use of underscore.js template engine. Thus you have free flexibility of the template engine by using placeholders, if controls and for loops. The component object itself is by default available as placeholder. So you may directly access all of the components properties and methods.

<h6>Using create method</h6>

Another and yet more powerful way to create the markup for the components representation is by using the <span class="code-hint">create</span> method. It provides three different parameters: The parent component object reference, the children count of the parent component and the child index of the current component.

So let's take again the Message component example and use the <span class="code-hint">create</span> rather than the <span class="code-hint">getTemplate</span> method.

<pre class="imp-code code-white line-numbers language-js">
	<code class="language-js">import AbstractComponent from '../../../../vendor/impulsephp/impulsebundle/src/Resources/assets/js/impulse-bundle/Impulse/UI/Components/AbstractComponent';
    
    export default class Message extends AbstractComponent {
        constructor() {
            super();
            this.message = '';
        }

        setMessage(message) {
            this.message = message;
        }

        create(parentComponent, childrenCount, childIndex) {
        	let node = this.client.createElementsFromStringWithUid(
                this.client.renderTemplate('<div><%= component.message %></div>', { component: this }),
                this.getUid()
            );
            
            // ... other code related to rendering the component
        
        	parentComponent.getParentWrapper(childIndex).append(node);
            return node;
        }
    }
    
    window.Message = Message;</code>
</pre>

Based on that implementation way you have the free flexibility of javascript code to create more complex components with ease.
