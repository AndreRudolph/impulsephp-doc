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

<h4><a name="file-downloads">File downloads</a></h4>
The client interface provides methods for providing file downloads to the user.
Below you will find two different variants of file downloads.

<h5><a name="static-file-downloads">Static file downloads</a></h5>
Static files mean already existing files that are publicly available by the webserver
and can be served for downloads. For this purpose, the client interface provides the method
<span class="code-hint">downloadFile</span>.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
use Impulse\ImpulseBundle\UI\Client\ClientInterface;

class AppController extends AbstractController
{
    public function afterCreate(ClientInterface $client, Event $event)
    {
        $client->downloadFile('/path/to/the/file', 'name_of_the_file.txt');
    }
}</code>
</pre>

The first parameter specifies the path together with the name of the file
that will be downloaded. The second, optional parameter, can be used if the 
provided download file will have another file name.