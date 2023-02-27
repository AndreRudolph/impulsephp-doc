<h3 class="doc-title">Client events</h3>

- [Introduction](#introduction)
- [Internal workflow](#internal-workflow)
- [Register listener](#register-listener)

<h4><a id="introduction">Introduction</a></h4>

Components are useless without the usage of event listeners reacting to client events.
However, since React does only allow one event listener per event dealing with multiple
event handlers becomes tricky. It becomes even more tricky because of how the Impulse
framework works and its components are designed. 

Luckily, the framework provides an easy but yet very flexible and powerful approach for
registering event listeners.

<h4><a id="internal-workflow">Internal workflow</a></h4>

Internally, each component holds a priority event listener map with the events as keys
and a list with registered listeners. Usually the listeners are not added directly but
instead the names of the later called methods. For this purpose the framework provides
a <span class="code-hint">Listener</span> decorator for registering listeners.

<h4><a id="register-listener">Register listener</a></h4>

Whenever you want to register a listener you should extend the parent's <span class="code-hint">
registerEventListener</span> method and register the decorators.

<pre class="imp-code code-white language-js code-xl">
<code class="language-js">import { AbstractReactComponent } from '@impulsephp/client-ts';

export class MyComponent extends AbstractReactComponent {
    @Listener([
        { event: 'click', method: 'handleClick', priority: 500 }
    ])
    registerEventListener() {
        super.registerEventListener();
    }

    public handleClick() {
        return (event, initiator, next) => {
            // apply your logic here
            next(event, initiator);
        };
    }
}</code>
</pre>