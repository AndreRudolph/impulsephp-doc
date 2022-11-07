<h3 class="doc-title">App & Configuration</h3>

- [Introduction](#introduction)
- [Typescript files](#typescript-files)
  - [app.js](#app-js)
  - [config.js](#config-js)
  - [AppBundle.js](#appbundle-js)
  - [AppComponentProvider.js](#appcomponentprovider-js)
- [SCSS files](#scss-files)
  - [app.scss](#app-scss)

<h4><a name="introduction">Introduction</a></h4>
This article gives you a brief overview of the assets provided by the framework.

<h4><a id="app-js">app.js</a></h4>

The app.js is the entry point of the Impulse javascript application. Its purpose is to create the Impulse App instance, sets the configuration (see section below) and to run it. 

<pre class="imp-code code-white  language-js">
<code class="language-js">import './../scss/app.scss';
import '@impulsephp/client-ts';
import ImpulseRuntime from './config';
import { AppBundle } from "./app/AppBundle";
import { App } from "../../../impulse-client-ts";

document.addEventListener("DOMContentLoaded", (event) => {
    let app = new App();
    app.setConfig(ImpulseRuntime);
    app.run();
});</code>
</pre>

<h4><a id="config-js">config.js</a></h4>

The config.js file provides basic configurations for the Impulse client runtime environment. The configuration style may
look familiar to you if you have worked with WebpackEncoreBundle previousely.

Below is the default configuration file.

<pre class="imp-code code-white  language-js">
<code class="language-js">import { Impulse } from '@impulsephp/client-ts';

let ImpulseRuntime: Impulse = Impulse.new();

ImpulseRuntime

    // enables the page main busy indicator. Otherwise no busy indicator will take effect for a server request. The
    // default indicator itself can be exchanged by calling registerBusyIndicator.
    .enableBusyIndicator()

    // disable the reset of scroll position after refresh
    // .disableScrollReset()

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

<h4><a id="appbundle-js">AppBundle.js</a></h4>

The AppBundle provides you some convenient methods to register component providers and registering new services as well
as replacing existing ones with your custom implementations.

<h4><a id="appcomponentprovider-js">AppComponentProvider.js</a></h4>

The AppComponentProvider class is the place where you can register your custom client component classes of your app.

<pre class="imp-code code-white  language-js">
<code class="language-js">import { ComponentClassRegistry } from '@impulsephp/client-ts';
import { ComponentProvider } from '@impulsephp/client-ts';

import { Message } from "./components/Message";

export class AppComponentProvider implements ComponentProvider
{
    boot(componentClassRegistry: ComponentClassRegistry)
    {
        componentClassRegistry.register(Message.name, Message);
    }
}</code>
</pre>