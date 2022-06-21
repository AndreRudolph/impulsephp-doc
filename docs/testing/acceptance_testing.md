<h3 class="doc-title">Functional testing</h3>

- [Introduction](#introduction)
- [Testing framework](#testing-framework)
- [Create a test](#create-tests)
    - [Records](#records)
    - [Response verificator](#response-verificator)

<h4><a id="introduction">Introduction</a></h4>
Functional testing is a common and in most cases one of the last layers in a testing waterfall model. Its purpose is to have a more high level perspective and expectations of the functions provided by the application.

In scope of the Impulse PHP Framework with functional tests we mean tests that are directly working with the implemented application by accessing its published URLs. So a test defines a collection of consecutive requests (we call it records) send to the application.

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

<h5><a id="records">records</a></h5>

A record is basically a collection of three different parts. 

<ul>
	<li>Request: The request itself with all required information that will be send to the application</li>
    <li>Response verificator: The response verificator can be used to analyze the response of the application to verify its correctness.</li>
    <li>Page model verificator: The page model verificator is used to verify if the internal page model of the application after the respective request is as expected. It is mostly used for the framework developers to verify the correctness of the internal page state but can also be used by the frameworks user for further verifications.</li>
</ul>

It is a good practice to have each record placed in its own method. The following code will run a single record by sending a GET request to the root URI with no verificators.

<pre class="code-white line-numbers language-php">
	<code class="imp-code language-php"><?php

	namespace App\Tests;

	// other imports
    use Impulse\ImpulseBundle\Tester\Record;
    use Impulse\ImpulseBundle\Tester\Request\RequestBuilder;

	class CounterWebTest extends WebTestCase
	{
        public function testCounter()
        {
            $client = static::createClient();
            $tester = new Tester($client);
			$tester->record();
            $tester->run();
            self::assertResponseIsSuccessful();
        }
        
        public function initialRequest(): Record
        {
        	$request = RequestBuilder::request('GET', '/')->create();
            return new Record($request, null, null);
        }
	}</code>
</pre>

Creating and defining more complex requests is covered later in this article.

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