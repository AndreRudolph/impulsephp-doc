<h3 class="doc-title">Middlewares</h3>

- [Introduction](#introduction)
- [Pre action middleware](#pre-action-middleware)
- [Post action middleware](#post-action-middleware)
- [Middleware context](#middleware-context)

<h4><a id="introduction">Introduction</a></h4>

Impulse middlewares are a great concept that are highly inspired by the Laravel Framework. Since the workflow 
of an application using the Impulse PHP Framework is different from classic frameworks, the concept is not identical.

Middlewares are a piece of code encapsulated in a class that can be executed either before a controller method will be executed (pre middleware) or after the methods exection (post middleware). Pre middlewares are mostly used for securing access to a controller. Post middlewares although have more use cases e.g. updating database records, clean up tasks, etc. that must take place right after a controllers execution cycle.

A middleware that just does nothing looks like the following.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
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

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
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

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
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

<pre class="imp-code  code-white language-php">
<code class="language-php">class MyController extends AbstractController
{
    #[PreMiddleware(LoggingMiddleware::class, context: ['method' => 'afterCreate']])]
    public function afterCreate(Event $event): void
    {
        parent::afterCreate($event);
    }
}</code>
</pre>

<div class="info-box info">In future the context parameters will be enriched by view arguments.</div>
