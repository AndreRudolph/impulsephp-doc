<h3 class="doc-title">Validation & states</h3>

- [Introduction](#introduction)
- [Component state](#component-state)
- [Validation](#validation)

<h4><a id="introduction">Introduction</a></h4>
Validation is a key requirement of any application when it comes to user's input. The Impulse framework offers some good mechanics to have a comfortable and yet flexible way of validating forms. 

<h4><a id="component-state">Component state</a></h4>

Every component that extends from <span class="code-hint">AbstractComponent</span> provides a built-in solution for a component state. In detail, the state comprises of three different attributes.

The state itself can be any name you need to have. By default there are four different states. 

- <span class="code-hint">true</span> which indicates that the component might have a state in the future and enables the state handling. The DOMs structure will be prepared for having a state and a state message but nothing is yet visualized. 
- <span class="code-hint">ok</span> which indicates that everything is fine
- <span class="code-hint">warning</span> which indicates that the input in general will be accepted but the user will be warned about potential side effects
- <span class="code-hint">error</span> which indicates an error that prevents further processing

<h4><a id="introduction">Introduction</a></h4>

Impulse middlewares are a great concept that are highly inspired by the Laravel Framework. Since the workflow 
of an application using the Impulse PHP Framework is different from classic frameworks, the concept is not identical.

Middlewares are a piece of code encapsulated in a class that can be executed either before a controller method will be executed (pre middleware) or after the methods exection (post middleware). Pre middlewares are mostly used for securing access to a controller. Post middlewares although have more use cases e.g. updating database records, clean up tasks, etc. that must take place right after a controllers execution cycle.

A middleware that just does nothing looks like the following.

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
	namespace App\Controller\Middleware;
	use Impulse\ImpulseBundle\Execution\Middleware\EventListenerMiddleware;

    class MySpecialMiddleware implements EventListenerMiddleware
    {
        public function execute(array $context): bool
        {
            return true;
        }
    }</code>
</pre>

So basically a middlware is identified by implementing the <span class="code-hint">Impulse\ImpulseBundle\Execution\Middleware\EventListenerMiddleware</span> interface. How a middleware will be applied to a specific controller method is captured in the next chapter.

<h5><a id="pre-action-middleware">Pre action middleware</a></h5>

Let's consider you want to have an often used action like checking if a user is authenticated and do not grant access if the user is not. That's a perfect example for a middleware that can be re-used for different methods
of the same controller or even for different controllers. 

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
	namespace App\Controller\Middleware;
	use Impulse\ImpulseBundle\Execution\Middleware\EventListenerMiddleware;
	use Psr\Container\ContainerInterface;
	use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

    class AuthenticationRequiredMiddleware extends AbstractController implements EventListenerMiddleware
    {
        public function __construct(ContainerInterface $container)
        {
            $this->container = $container;
        }

        public function execute(array $context): ?Redirect
        {
            if (!$this->isGranted('IS_AUTHENTICATED_FULLY')) {
                return false;
            }

            return null;
        }
    }</code>
</pre>

Let's take this middleware and register it for a specific controller method.

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

    use App\Controller\Middleware\AuthenticationRequiredMiddleware;
    use Impulse\ImpulseBundle\Controller\AbstractController;
    use Impulse\ImpulseBundle\Controller\Annotations\PreMiddleware;
    use Impulse\ImpulseBundle\Execution\Events\Event;


    class MyController extends AbstractController
    {
        #[PreMiddleware(AuthenticationRequiredMiddleware::class)]
        public function afterCreate(Event $event): void
        {
            parent::afterCreate($event);
        }
    }</code>
</pre>

As you can see, the middleware is registered by a <span class="code-hint">Impulse\ImpulseBundle\Controller\Annotations\PreMiddleware</span> property (annotation) and thus will be executed before the controller method itself will be executed. Since the middleware will return false in case of no authentication, the controller method will not be executed in that case.

<h5><a id="post-action-middleware">Post action middleware</a></h5>
Post action middlewares must implement the interace <span class="code-hint">Impulse\ImpulseBundle\Execution\Middleware\EventListenerMiddleware</span> aswell like pre middlewares but are registered with a <span class="code-hint">Impulse\ImpulseBundle\Controller\Annotations\PreMiddleware</span> property (annotation). 

<h5><a id="middlware-context">Middleware context</a></h5>

The interface <span class="code-hint">Impulse\ImpulseBundle\Execution\Middleware\EventListenerMiddleware</span> requires an implementation of the method execute along with an array parameter <span class="code-hint">$context</span>. This context array can have values that have been set to a middleware registration.

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
	<code class="imp-code language-php">class MyController extends AbstractController
    {
        #[PreMiddleware(LoggingMiddleware::class, context: ['method' => 'afterCreate']])]
        public function afterCreate(Event $event): void
        {
            parent::afterCreate($event);
        }
    }</code>
</pre>

<div class="info-box info">In future the context parameters will be enriched by view arguments.</div>
