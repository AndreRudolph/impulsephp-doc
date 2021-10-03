<h3 class="doc-title">Websockets connections</h3>

- [Connection handling](#connection-handling)
- [Retrieve connections](#retrieve-connections)
- [Retrieve own connection](#retrieve-own-connection)

<a name="connection-handling">Connection handling</a>

In websocket context the websocket server needs to keep track of all registeres connections and provides convenient methods to access a specific set of connections. For this purpose, you can use an instance of the ConnectionHandler class.

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
  <pre class="code-white imp-code line-numbers language-php">
	<code class="language-php"><?php

    namespace App\Controller\Websocket;

    use Impulse\ImpulseBundle\Controller\AbstractController;
    use Impulse\ImpulseWebsocketBundle\Websockets\Connection;
    use Impulse\ImpulseWebsocketBundle\Websockets\ConnectionHandler;

    class TestController extends AbstractController
    {
        #[Transient] private ConnectionHandler $connectionHandler;

        public function __construct(ConnectionHandler $connectionHandler)
        {
            $this->connectionHandler = $connectionHandler;
        }
    }</code>
  </pre>
</div>

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
  
  Once you enabled the websocket communication, the Impulse framework automatically tries to establish a websocket connection after the initial request has been processed and rendered by the client. If the server is not started, the client proceeds as usual without websockets and works instead with AJAX requests. You can start the websocket server by executing the following command.

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
	<code class="language-bash">php bin/console impulse_websocket:wsserver</code>
  </pre>
</div>
