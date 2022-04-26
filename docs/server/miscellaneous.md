<h3 class="doc-title">Middlewares</h3>

- [Introduction](#introduction)
- [File downloads](#file-downloads)

<h4><a id="introduction">Introduction</a></h4>

<h5><a id="pre-action-middleware">File downloads</a></h5>

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
