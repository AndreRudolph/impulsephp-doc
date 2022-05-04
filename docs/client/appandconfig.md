<h3 class="doc-title">App & Configuration</h3>

- [Introduction](#introduction)
- [app.js](#app-js)
- [config.js](#config-js)

<h4><a id="introduction">Introduction</a></h4>

Managing complex applications becomes tricky when dealing with assets like javascript, sass / scss and static resources like images or media in general. To reduce this complexity, the Impulse PHP Framework takes advantage of using webpack as a module builder to compile, minify and collect all required assets together. 

Luckily the Symfony framework already provides the awesome WebpackEncoreBundle that wraps webpack by providing a handy facade for its configuration.

Once the Impulse framework has been initialized, the original webpack.config.js had been replaced and all files in the assets/ directory had been removed.

<h4><a id="app-js">app.js</a></h4>

The app.js is the entry point of the Impulse javascript application. Its purpose is to create the Impulse App instance, sets the configuration (see section below) and to run it. 

<pre class="imp-code code-white line-numbers language-js">
	<code class="language-js">import './../../vendor/impulsephp/impulsebundle/src/Resources/assets/js/impulse-bundle/ImpulseCore';
	import './../../vendor/impulsephp/impulsebundle/src/Resources/assets/js/impulse-bundle/ImpulseComponents';
	import './app/AppComponents';
	import './../scss/app.scss';

    import App from './../../vendor/impulsephp/impulsebundle/src/Resources/assets/js/impulse-bundle/Impulse/Kernel/App';
    import ImpulseRuntime from './config';

    document.addEventListener("DOMContentLoaded", (event) => {
        let app = new App();
        app.setConfig(ImpulseRuntime);
        app.run();
    });</code>
</pre>

As you might have noticed, the sources are not copied to the assets/ directory and rather being imported from the ImpulseBundle. The main reason is that the user of the framework does not need to manually copy the source files by hand with every framework update and thus to minimize compatibility issues with newer versions.

<h6><a id="config-js">config.js</a></h6>

The previous mentioned WebpackEncoreBundle provides an awesome configuration facade for webpack configuration. We decided to use the same approach for configuring the Impulse client engine by providing a fluent interface API.

The example below is the default configuration file.

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
