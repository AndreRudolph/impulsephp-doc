<h3 class="doc-title">Middlewares</h3>

- [Introduction](#introduction)
- [File downloads](#file-downloads)

<h4><a id="introduction">Introduction</a></h4>

<h5><a id="pre-action-middleware">File downloads</a></h5>

Providing files is a common task that an application might has to provide. For providing static files, an anchor can be used. For providing dynamic files like current statistics or reports, the Impulse PHP Framework provides a convenient method to provide a file download.

For this purpose you can simply call the <span class="code-hint">provideDownload</span> method inherited from the <span class="code-hint">AbstractController</span>. 

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
