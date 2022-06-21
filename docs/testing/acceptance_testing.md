<h3 class="doc-title">Functional testing</h3>

- [Introduction](#introduction)
- [Testing framework](#testing-framework)
- [Create a test](#create-tests)
    - [Records](#records)
    - [Custom handling](#custom-handling)
    - [Available rules](#available-rules)

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

There are no recordings defined yet. In the next chapter you'll see how recordings will be created.

<h5><a id="records">records</a></h5>
