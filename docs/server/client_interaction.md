<h3 class="doc-title">Client interaction</h3>

- [Introduction](#introduction)
- [Redirects](#redirects)
- [File downloads](#file-downloads)
    - [Static files](#static-file-downloads)
    - [Dynamic files](#dynamic-file-downloads)
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

<h5><a name="static-file-downloads">Static files</a></h5>
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

<h5><a name="dynamic-file-downloads">Dynamic files</a></h5>
Providing dynamically generated one time files like reports or statistics for download is a common task for web applications.
Hence the client interface provides a method with name <span class="code-hint">downloadRaw</span>.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
use Impulse\ImpulseBundle\UI\Client\ClientInterface;

class AppController extends AbstractController
{
    public function afterCreate(ClientInterface $client, Event $event)
    {
        $client->downloadRaw('file.txt', 'Content of the file');
    }
}</code>
</pre>

The first parameter defines the name of the file that will be generated
along with its content as second parameter. There is an optional third parameter
that declares the type of the content. By default it is set to <span class="code-hint">application/octet-stream</span>.

<h4><a name="scrolling">Scrolling</a></h4>
With the client interface you are also able to send commands to client to change the current scroll position.

<h5><a name="scroll-to-position">Scroll to position</a></h5>
In case you want to send a command to the browser to scroll to a specific position,
you can use the <span class="code-hint">scrollTo(x, y)</span> method.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
use Impulse\ImpulseBundle\UI\Client\ClientInterface;

class AppController extends AbstractController
{
    public function afterCreate(ClientInterface $client, Event $event)
    {
        $client->scrollTo(0, 0);
    }
}</code>
</pre>

The code above will scroll the client's browser to the top left corner.
You may also leave out one coordinate and set it to null in order to do not
scrolling for that specific coordinate.