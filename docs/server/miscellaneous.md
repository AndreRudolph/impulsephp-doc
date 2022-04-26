<h3 class="doc-title">Middlewares</h3>

- [Introduction](#introduction)
- [File downloads](#file-downloads)

<h4><a id="introduction">Introduction</a></h4>

<h5><a id="pre-action-middleware">File downloads</a></h5>

Providing dynamically generated one time files like reports or statistics for download is a common task for web applications. Hence the <span class="code-hint">AbstractController</span> implements a convenient method called <span class="code-hint">provideDownload</span>. 

  <pre class="code-white line-numbers language-php">
  	<code class="imp-code language-php"><?php
  	namespace App\Controller;
  	use Impulse\ImpulseBundle\Controller\AbstractController;
    use Impulse\ImpulseBundle\Controller\Annotations\Listen;
    use Impulse\ImpulseBundle\Events\Events;
    use Impulse\ImpulseBundle\Execution\Events\Event;

  	class AppController extends AbstractController
  	{
		#[Listen(event: Events::CLICK, component: 'btnCreateReport')]   
        public function createReport(Event $event)
        {
        	$payload = 'This is going to be the content of the file';
            $this->provideDownload('generaredFile.txt', $payload, 'text/plain');
        }
  	}</code>
  </pre>
  
  Note that the third parameter is the content type of the file and is optional. If no content type is declared, application/octet-stream is used by default.