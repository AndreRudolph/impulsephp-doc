<h3 class="doc-title">App & Configuration</h3>

- [Introduction](#introduction)
- [app.js](#app-js)
- [config.js](#config-js)

<h4><a id="introduction">Introduction</a></h4>

For dealing with assets, the Impulse PHP Framework advises the usage of webpack. Webpack is a module bundler that compiles, minifies and collects all required web assets like javascript, scss and static resources such as images altogether.  

Every server component is mapped to a client side component. This article covers the basics for rendering and updating components aswell as creating and handling event listeners.

However, once the framework has been successfully intiailized, all required assets are placed in the assets/ directory.

<h4><a id="app-js">app.js</a></h4>

The app.js is the entry point of the complete javascript application. Its purpose is to create the Impulse App instance, sets the configuration (see section below) and to run it. 

<h6><a id="config-js">config.js</a></h6>

The config.js file follows a similar approach like the webpack.config.js. It defines some configurations by using a fluent interface with convenient methods. The very basics are provided by default and can be configured by using the configuration API.

<pre class="imp-code code-white line-numbers language-js">
	<code class="language-js">import Impulse from './../../vendor/impulsephp/impulsebundle/src/Resources/assets/js/impulse-bundle/Impulse/Kernel/Impulse';

let ImpulseRuntime = Impulse.new();

ImpulseRuntime

    // enables the page main busy indicator. Otherwise no busy indicator will take effect for a server request. The
    // default indicator itself can be exchanged by calling registerBusyIndicator.
    .enableBusyIndicator()

    // overwrites the default page busy indicator implementation.
    // .registerBusyIndicator(new MyBusyIndicatorClass())

    // registers a new request listener.
    // .registerRequestListener(new MyRequestListener())

    // registers a new response listener.
    // .registerResponseListener(new MyResponseListener())

    // sets the timeout for each server request for the underlying axios library. The value 0 means that there is no timeout.
    .setRequestTimeout(0);

export default ImpulseRuntime;</code>
</pre>
