<h3 class="doc-title">Components</h3>

- [Introduction](#introduction)
- [Create components](#create-components)
- [Updates from server](#updates-from-server)
- [Component lifecycle](#component-lifecycle)
- [Event listener](#event-listener)
- [Learn more about components](#advanced_topics)

<h4><a id="introduction">Introduction</a></h4>

Every server component is mapped to a client side component. This article covers the basics for rendering and updating components aswell as creating and handling event listeners.

<h5><a id="create-components">Create custom component</a></h5>

As a foreword, every client implementation of components should extend the <span class="code-hint">AbstractComponent</span> class. This ensures a correct setup and lifecycle for that component. Please only do not extend the class if you absolutely know what you need to do.

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

For very simple components the <span class="code-hint">getTemplate</span> method was introduced. It is used by the base <span class="code-hint">create</span> method implementation of <span class="code-hint">AbstractComponent</span> class that automatically knows where in the DOM your component must be placed. A custom implementation of <span class="code-hint">create</span> method will be covered in the next chapter.

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

The code snippet <span class="code-hint">parentComponent.getParentWrapper(childIndex).append(node);</span> is required in order to retrieve the insertion point from the parent component where the message component should be appended to.

This approach has some advantages since it allows to create more complex components with ease. You are free in pass any template arguments you and or to not even use the template engine and rather create the components by manually with the javascript API.

<h5><a id="updates-from-server">Updates from server</a></h5>

Usually non static components like buttons, tabboxes, etc. can be updated on server side and the changes are later populated to the client. When a property change is populated from the server, the client component implementation requires a setter method to store the updated value inside the component and further to update its appeareance if needed.

As a simple example we can again use the message component as an example and change the <span class="code-hint">setMessage</span> method to also recognize updates.

<pre class="imp-code code-white line-numbers language-js">
	<code class="language-js">export default class Message extends AbstractComponent {
        // ... constructor

        setMessage(message) {
            this.message = message;
            if (this.isAttached()) {
            	this.getHtmlComponent().innerHTML = message;
            }
        }

        // ... create method
    }
    
    window.Message = Message;</code>
</pre>

The setter was changed that is checks whether the component has already been rendered (isAttached) and updates the inner html text with the new message value. The <span class="code-hint">getHtmlComponent</span> method usually returns the HTML node that is the root tag of the components DOM.
