<h3 class="doc-title">WebsocketBundle installation</h3>

- [Introduction](#introduction)
- [Installation](#installation)
- [Server startup](#server-startup)

<a name="introduction"></a>

Websockets is basically a network protocol allowing bidirectional communication between client and server. Once connected, the connection is usually kept open until the client manually closes the connection by refreshing or closing the browser tab. The main difference between conventional HTTP communication is, that the client does not need to request for an update or a notification. Instead, the server is able to continousely send updates to the client. 

With that being sad, by processing a client request (such as sending a message in a chat room) other available clients can be informed about that message too and display it **_without explicitly_** requesting it.


<h4><a id="installation">Installation</a></h4>

Installation of the ImpulseWebsocketBundle is pretty easy by adding it to composer require command.

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
          <div class="button red"></div>
          	<div class="button yellow"></div>
          	<div class="button green"></div>
        </div>
    </div>
  </div>
  <pre class="code-white imp-code line-numbers language-shell">
	<code class="language-bash">composer require impulsephp/impulsewebsocketbundle</code>
  </pre>
</div>

If not provided automatically, you need to create the config file impulse_websockets.yaml inside the config/packages directory for the websocket settings. The file contains the following default settings:

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
          <div class="button red"></div>
          	<div class="button yellow"></div>
          	<div class="button green"></div>
        </div>
    </div>
  </div>
  <pre class="code-white imp-code line-numbers language-yaml">
	<code class="language-yaml">impulse_websockets:
		enabled: true
		protocol: 'ws'
		host: 'localhost'
		port: 7777</code>
  </pre>
</div>

After that, you need to extend the App's ImpulseController by the one provided by the ImpulseWebsocketBundle.

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
          <div class="button red"></div>
          	<div class="button yellow"></div>
          	<div class="button green"></div>
        </div>
    </div>
  </div>
  <pre class="code-white line-numbers language-php">
  	<code class="imp-code language-php"><?php
	namespace App\Controller;

	class ImpulseController extends \Impulse\ImpulseWebsocketBundle\Controller\ImpulseController
	{

	}</code>
  </pre>
</div>

The last step is to include the js ImpulseWebsocketBundle by copying it from the Bundle and place it inside the assets/js directory and import it in the app.js

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
          <div class="button red"></div>
          	<div class="button yellow"></div>
          	<div class="button green"></div>
        </div>
    </div>
  </div>
  <pre class="code-white line-numbers language-js">
  	<code class="imp-code language-js">import './impulse-bundle/ImpulseCore';
	import './impulse-bundle/ImpulseComponents';
	import './impulse-websocket-bundle/ImpulseWebsocketBundleEntry';

	import './app/homepage/AppComponents';

	import './../scss/app.scss';

    document.addEventListener("DOMContentLoaded", function(event) {
        let app = new App();
        app.run();
    });</code>
  </pre>
  
  <h4><a id="server-startup">Server startup</a></h4>
  
  Once you enabled the websocket communication, the Impulse framework automatically tries to establish a websocket connection after the initial request has been processed and rendered by the client.
