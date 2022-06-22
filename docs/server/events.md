<h3 class="doc-title">Events</h3>

- [Introduction](#introduction)
- [Global listener](#global-listener)
	- [Register listener](#register-listener)
    - [Broadcast event](#broadcast-event)
    - [Unsubscribe listener](#unsubscribe-listener)


<h4><a id="introduction">Introduction</a></h4>
Event based architectures are a good and common practice to separate concerns and responsibilities between event emitter and event listener. In the previous documentations of controllers and components you have learned how event listeners can be easily added to your application.

However, this article explains events more in depth and will also cover the topic of page global event listener.

<h4><a id="global-listener">Global listener</a></h4>

<h4><a id="testing-framework">Testing framework</a></h4>

The Impulse framework provides a testing framework that has been especially designed for creating and executing functional tests. It is based on the awesome testing library provided by the Symfony framework. You don't need to install any further dependencies since they are installed by default with the Impulse framework.

<h4><a id="create-tests">Create tests</a></h4>

The example below shows the skeleton for any functional test that requires a client and a tester instance.

<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php

	namespace App\Tests;

	use Impulse\ImpulseBundle\Components\UidHelperTrait;
	use Impulse\ImpulseBundle\Events\Events;
	use Impulse\ImpulseBundle\Tester\Record;
	use Impulse\ImpulseBundle\Tester\Request\Command;
	use Impulse\ImpulseBundle\Tester\Request\RequestBuilder;
	use Impulse\ImpulseBundle\Tester\Response\Response;
	use Impulse\ImpulseBundle\Tester\Tester;
	use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

	class CounterWebTest extends WebTestCase
	{
        public function testCounter()
        {
            $client = static::createClient();
            $tester = new Tester($client);

            // add recordings here

            $tester->run();
            self::assertResponseIsSuccessful();
        }
	}</code>
</pre>

There are no recordings defined yet that can be executed. In the next we'll explain recordings in depth.

<h5><a id="records">Records</a></h5>

A record is basically a compound of three different parts that will be explained below.

<h6>Request</h6>
The request itself with all required information that will be send to the application. The request is mandatory within the request.

<h6>Response verificator</h6>
The response verificator can be used to analyze the response of the application to verify its correctness. The response verificator is optional.

<h6>Page model verificator</h6>
The page model verificator is used to verify if the internal page model of the application after the respective request is as expected. It is mostly used for the framework developers to verify the correctness of the internal page state but can also be used by the frameworks user for further verifications. The page model verificator is optional.

It is a good practice to have each record placed in its own method. The following code will run a single record by sending a GET request to the root URI with no verificators.

<h4><a id="requests">Requests</a></h4>
The first part of any record is the request that will be send to the server. The testing framework offers a Facade for creating requests for convenient use cases.

<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php

	namespace App\Tests;

	// other imports
    use Impulse\ImpulseBundle\Tester\Record;
    use Impulse\ImpulseBundle\Tester\Request\RequestBuilder;

	class CounterWebTest extends WebTestCase
	{
        public function initialRequest(): Record
        {
        	$request = RequestBuilder::request('GET', '/')->create();
            return new Record($request, null, null);
        }
	}</code>
</pre>

Every request after the first request should be an AJAX request. For this purpose, the RequestBuilder offers an ajax method.

<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php

	namespace App\Tests;

	// other imports
    use Impulse\ImpulseBundle\Tester\Record;
    use Impulse\ImpulseBundle\Tester\Request\RequestBuilder;

	class CounterWebTest extends WebTestCase
	{
        public function ajaxRequest(): Record
        {
        	$request = RequestBuilder::ajax('POST', '/_impulse/event/')
            	->create();
           
            return new Record($request, null, null);
        }
	}</code>
</pre>

<h5><a id="component-events">Component events</a></h5>

A real world scenario test would not just send an ajax request without any content but instead would e.g. simulate a click an a specific button inside your application. Consider having a counter app that increases a displayed counter whenever the user clicks on the button with the id (remember: the id is the components internal id and not the DOM id) btnIncrease.

To achieve this, the RequestBuilder offers the <span class="code-hint">command</span> method.

<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php

	namespace App\Tests;

	// other imports
    use Impulse\ImpulseBundle\Events\Events;
    use Impulse\ImpulseBundle\Tester\Request\Command;

	class CounterWebTest extends WebTestCase
	{
        public function ajaxRequest(): Record
        {
        	$request = RequestBuilder::ajax('POST', '/_impulse/event/')
            	->command(new Command(Events::CLICK, null, 'btnIncrease'))
            	->create();
           
            return new Record($request, null, null);
        }
	}</code>
</pre>

<h5><a id="component-modifications">Component modifications</a></h5>

Like component events, it is easy to provide component modifications for a request (like simulating an input for a textbox). By calling the <span class="code-hint">component</span> method of the <span class="code-hint">RequestBuilder</span> you can simply pass values as component modifications.

<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php

	namespace App\Tests;

	// other imports
    use Impulse\ImpulseBundle\Events\Events;
    use Impulse\ImpulseBundle\Tester\Request\Command;

	class CounterWebTest extends WebTestCase
	{
        public function ajaxRequest(): Record
        {
        	$request = RequestBuilder::ajax('POST', '/_impulse/event/')
            	->component('textboxId', 'value', 'my entered value')
            	->create();
           
            return new Record($request, null, null);
        }
	}</code>
</pre>

<h4><a id="verification">Verification</a></h4>

<h5><a id="response-verificator">Response verificator</a></h5>
A response verificator is a closure function with two arguments: The current response object and (if not the first request) the previous response object. You may use common PHPUnit assertions to verify the response. See the example below for a simple status code verification.

<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php

	namespace App\Tests;

	// imports

	class CounterWebTest extends WebTestCase
	{
        public function initialRequest(): Record
        {
        	$request = RequestBuilder::request('GET', '/')->create();
            
            $responseVerificator = function(Response $response, ?Response $previousResponse)
            {
            	$this->assertEquals(200, $response->getStatusCode());
            };
            
            return new Record($request, responseVerificator, null);
        }
	}</code>
</pre>

Please note that the initial response will return a html document (since its the entry point of the application) whereas consequtive requests will only return json response.

<h5><a id="page-model-verificator">Page model verificator</a></h5>
A page model verificator is like the response verificator a closure function with the exact same parameters. You could use one verificator for both types of verifications but it is a good practice to separate concerns.

<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php

	namespace App\Tests;

	// imports

	class CounterWebTest extends WebTestCase
	{
        public function initialRequest(): Record
        {
        	$request = RequestBuilder::request('GET', '/')->create();
                       
            $pageModelVerificator = function(Response $response, ?Response $previousResponse)
            {
            	$pageModel = $response->getPageModel();
                $this->assertTrue($pageModel->hasComponentWithId('idOfComponent'));
            }
                       
            return new Record($request, null, null);
        }
	}</code>
</pre>

For sure you can do more complex verifications. For a full list of available methods of the response and the page model object, you can have a look into the class.
