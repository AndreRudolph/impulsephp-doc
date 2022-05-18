<h3 class="doc-title">Miscellaneous</h3>

- [Static file downloads](#static-file-downloads)
- [Dynamic file downloads](#dynamic-file-downloads)

<h5><a id="static-file-downloads">Static file downloads</a></h5>
In web applications it is a common use case to provide static file downloads such as media, manuals, licenses, etc. that are served from the web server. To achieve this, the <span class="code-hint">AbstractController</span> provides a convenient method called <span class="code-hint">downloadFile</span>.

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
            $this->downloadFile('path/to/file.ext', 'fileName.ext');
        }
  	}</code>
  </pre>
  
The first parameter must be a publicly available path for the static file. The second parameter will be the name of the file that will be downloaded on the user's device.

<h5><a id="dynamic-file-downloads">Dynamic file downloads</a></h5>

Providing dynamically generated one time files like reports or statistics for download is a common task for web applications. Hence the <span class="code-hint">AbstractController</span> implements a convenient method called <span class="code-hint">downloadRaw</span> which takes the name of the file, it's content (string) and an optional content type.

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
  
If no content type is declared, <span class="code-hint">application/octet-stream</span> is used by default.
