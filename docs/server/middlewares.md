<h3 class="doc-title">Middlewares</h3>

- [Introduction](#introduction)
- [Pre action middleware](#pre-action-middleware)
- [Post action middleware](#post-action-middleware)
- [Middleware context](#middleware-context)
    - [Server state](#server-state)
    - [Client state](#client-state)
    - [Reactivation](#reactivation)
	- [Available components](#registered_components)

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

    class AuthenticationRequiredMiddleware implements EventListenerMiddleware
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
