<h3 class="doc-title">Client interaction</h3>

- [Introduction](#introduction)
- [Redirects](#redirects)
- [File downloads](#file-downloads)
    - [Static file downloads](#static-file-downloads)
    - [Dynamic file downloads](#dynamic-file-downloads)
- [Scrolling](#scrolling)
    - [Scroll to position](#scroll-to-position)
    - [Scroll up](#scroll-up)
- [Import resources](#importing-resources)
- [Javascript response](#javascript-response)

<h4 name="introduction">Introduction</h4>

Impulse provides a client interface for common actions that will
take place at client side, such as redirects, scrolling, file downloads
as well as many other actions.

<h4><a id="redirects">Redirects</a></h4>
Redirects can be simply achieved by using the <span class="code-hint">redirect</span> 
method of the client interface.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
use Impulse\ImpulseBundle\UI\Client\ClientInterface;

class AppController extends AbstractController
{
    public function afterCreate(ClientInterface $client, Event $event)
    {
        $client->redirect('route_name', []);
    }
}</code>
</pre>
