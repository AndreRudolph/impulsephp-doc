<h3 class="doc-title">Miscellaneous</h3>

- [Dynamic file downloads](#dynamic-file-downloads)

<h5><a id="dynamic-file-downloads">Dynamic file downloads</a></h5>

Providing dynamically generated one time files like reports or statistics for download is a common task for web applications. Hence the <span class="code-hint">AbstractController</span> implements a convenient method called <span class="code-hint">downloadRaw</span>. 

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
            $this->downloadRaw('generaredFile.txt', $payload, 'text/plain');
        }
  	}</code>
  </pre>
  
  Note that the third parameter is the content type of the file and is optional. If no content type is declared, <span class="code-hint">application/octet-stream</span> is used by default.
