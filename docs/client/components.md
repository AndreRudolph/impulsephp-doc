<h3 class="doc-title">Components</h3>

- [Introduction](#introduction)
- [Create custom components](#create_custom_components)
- [Lifecycle](#lifecycle)
- [Learn more about components](#advanced_topics)

<h4><a id="introduction">Introduction</a></h4>

This article covers the basics of client implementation of components. In the "Learn about more components" section you will find further advanced topics related to components.

Note that everything related to client needs to have a complete working npm environment as described in the installation manual.

<h5><a id="custom-components">Create custom component</a></h5>

Creating the client side of components is quite easy. Let's take again that javascript example from the server side documentation of components:

<pre class="imp-code code-white line-numbers language-js">
	<code class="language-js">import AbstractComponent from '../../../../vendor/impulsephp/impulsebundle/src/Resources/assets/js/impulse-bundle/Impulse/UI/Components/AbstractComponent';
    
    export default class Message extends AbstractComponent {
        constructor() {
            super();
            this.message = '';
        }

        setMessage(message) {
            this.message = message;
            if (this.isAttached()) {
                this.getHtmlComponent().innerHTML = this.message;
            }
        }

        getTemplate() {
            return '<div><%= component.message %></div>';
        }
    }
    
    window.Message = Message;</code>
</pre>

Above is the definition of a Message component that shall print a text inside a div container. 