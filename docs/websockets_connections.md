<h3 class="doc-title">Websockets connections</h3>

- [Connection handling](#connection-handling)
	- [Retrieve all connections](#retrieve-all-connections)
	- [Retrieve own connection](#retrieve-own-connection)
- [WebsocketPageService](#websocket-page-service)
	- [Get component by id](#getComponentById)
    - [Get component by uid](#getComponentByUid)
    - [Import view](#importView)
    - [Send updates](#sendUpdates)
    - [Send native](#sendNative)

<h4><a name="connection-handling">Connection handling</a></h4>

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
  <pre class="code-white imp-code  language-php">
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

<h5><a id="retrieve-all-connections">Retrieve all connections</a></h5>

As previousely mentioned, the ConnectionHandler class provides a method to retrieve all connections that are registered in the websocket server.

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
  <pre class="code-white imp-code  language-php">
	<code class="language-php"><?php

    namespace App\Controller\Websocket;

    use Impulse\ImpulseBundle\Controller\AbstractController;
    use Impulse\ImpulseBundle\Execution\Events\Event;
    use Impulse\ImpulseWebsocketBundle\Websockets\Connection;
    use Impulse\ImpulseWebsocketBundle\Websockets\ConnectionHandler;

    class TestController extends AbstractController
    {
        #[Transient] private ConnectionHandler $connectionHandler;

        public function __construct(ConnectionHandler $connectionHandler)
        {
            $this->connectionHandler = $connectionHandler;
        }
        
        public function afterCreate(Event $event)
        {
        	$connections = $this->connectionHandler->getConnections();
            foreach ($connections as $connection) {
            	// do stuff here
            }
        }
    }</code>
  </pre>
</div>

<h5><a id="retrieve-own-connection">Retrieve own connection</a></h5>

Often you don't want to update your own client but instead just inform all the other clients. For that purpose, **_every_** event object passed to an Impulse controller contains a page object and keeps a reference to the connection object which triggered the event.

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
  <pre class="code-white imp-code  language-php">
	<code class="language-php"><?php

    namespace App\Controller\Websocket;

    use Impulse\ImpulseBundle\Controller\AbstractController;
    use Impulse\ImpulseWebsocketBundle\Websockets\Connection;
    use Impulse\ImpulseWebsocketBundle\Websockets\ConnectionHandler;

    class TestController extends AbstractController
    {
        // ...
        
        #[Listen(event: Events::CLICK, component: 'btnWebsocketMessage')]
        public function onWebsocketMsg(ClickEvent $event)
        {
            $connections = $this->connectionHandler->getConnections();
            foreach ($connections as $connection) {
                if ($connection === $event->getPage()->getConnection()) {
                    continue;
                }
               
                // do stuff for all other connections
            }
        }
    }</code>
  </pre>
</div>

<h4><a name="websocket-page-service">WebsocketPageService</a></h4>

The WebsocketPageServiceInterface provides the most convenient methods that are required to interact with a page of a specific connection. The following sub chapters explains these methods more in detail.

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
  <pre class="code-white imp-code  language-php">
	<code class="language-php"><?php

    namespace App\Controller\Websocket;

    use Impulse\ImpulseBundle\Controller\AbstractController;
    use Impulse\ImpulseWebsocketBundle\Websockets\Connection;
    use Impulse\ImpulseWebsocketBundle\Websockets\ConnectionHandler;
    use Impulse\ImpulseWebsocketBundle\Websockets\WebsocketPageServiceInterface;

    class TestController extends AbstractController
    {
        #[Transient] private ConnectionHandler $connectionHandler;
        #[Transient] private WebsocketPageServiceInterface $wsPageService;

        public function __construct(
        	ConnectionHandler $connectionHandler, 
        	WebsocketPageServiceInterface $wsPageService)
        {
            $this->connectionHandler = $connectionHandler;
            $this->wsPageService = $wsPageService;
        }
    }</code>
  </pre>
</div>

<h5><a id="getComponentById">Get component by id</a></h5>

You can retrieve a component by a specific id from a specific connection by using the **_getComponentById_** method.

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
  <pre class="code-white imp-code  language-php">
	<code class="language-php"><?php

    namespace App\Controller\Websocket;

    // imports

    class TestController extends AbstractController
    {
        // ...
        
        #[Listen(event: Events::CLICK, component: 'btnWebsocketMessage')]
        public function onWebsocketMsg(ClickEvent $event)
        {
            $connections = $this->connectionHandler->getConnections();
            foreach ($connections as $connection) {
                if ($connection === $event->getPage()->getConnection()) {
                    continue;
                }

    			$wndRoot = $this->wsPageService->getComponentById($connection, 'wndRoot');
                // do further stuff
            }
        }
    }</code>
  </pre>
</div>


<h5><a id="getComponentByUid">Get component by uid</a></h5>

You can retrieve a component by a specific id from a specific connection by using the **_getComponentByUid_** method.

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
  <pre class="code-white imp-code  language-php">
	<code class="language-php"><?php

    namespace App\Controller\Websocket;

    // imports

    class TestController extends AbstractController
    {
        // ...
        
        #[Listen(event: Events::CLICK, component: 'btnWebsocketMessage')]
        public function onWebsocketMsg(ClickEvent $event)
        {
            $connections = $this->connectionHandler->getConnections();
            foreach ($connections as $connection) {
                if ($connection === $event->getPage()->getConnection()) {
                    continue;
                }

    			$component = $this->wsPageService->getComponentByUid($connection, 'componentUid');
                // do further stuff
            }
        }
    }</code>
  </pre>
</div>

<h5><a id="importView">Import view</a></h5>
<h5><a id="sendUpdates">sendUpdates</a></h5>
<h5><a id="sendNative">sendNative</a></h5>