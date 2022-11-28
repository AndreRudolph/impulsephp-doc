<h3 class="doc-title">Responses</h3>

- [Introduction](#introduction)
- [Reserved namespaces](#reserved-namespaces)
- [Dynamic file downloads](#dynamic-file-downloads)

<h4><a id="introduction">Introduction</a></h4>
In most other, non ajax based, frameworks you as a developer are responsible for collecting and handling all required 
response(s) to the client, including rendering HTML markup, process redirects, provide file downloads, etc. 

However, while the Impulse PHP Framework automatically sends the required information for rendering components to the 
client. Apart from that, a pre-defined set of custom responses is provided to make life easier for dealing with
redirects, file downloads and even importing scripts and stylesheets on the fly.

<h5><a id="static-file-downloads">Static file downloads</a></h5>
In web applications it is a common use case to provide static file downloads such as media, manuals, licenses, etc. that are served from the web server. To achieve this, the <span class="code-hint">AbstractController</span> provides a convenient method called <span class="code-hint">downloadFile</span>.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
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

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
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
