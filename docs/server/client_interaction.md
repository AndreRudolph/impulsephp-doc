<h3 class="doc-title">Client interaction</h3>

- [Introduction](#introduction)
- [Redirects](#redirects)
- [Change URL](#change-url)
- [File downloads](#file-downloads)
    - [Static files](#static-file-downloads)
    - [Dynamic files](#dynamic-file-downloads)
- [Scrolling](#scrolling)
    - [Scroll to position](#scroll-to-position)
    - [Scroll up](#scroll-up)
- [Execute javascript](#execute-javascript)
- [Log messages](#log-messages)
- [Alert messages](#alert-messages)
- [Import resources](#importing-resources)
    - [Import css](#import-css)
    - [Import javascript](#import-javascript)

<h4 name="introduction">Introduction</h4>

Impulse provides a client interface for common actions that will
take place at client side, such as redirects, scrolling, file downloads
as well as many other actions. An object of this interface will be passed
to every <span class="code-hint">afterCreate</span> and every event listener
method as first argument.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
use Impulse\ImpulseBundle\UI\Client\ClientInterface;

class AppController extends AbstractController
{
    public function afterCreate(ClientInterface $client, Event $event)
    {
        
    }
}</code>
</pre>

<h4><a id="redirects">Redirects</a></h4>
Redirects can be simply achieved by using the <span class="code-hint">redirect</span> 
method of the client interface.

<pre class="imp-code code-white language-php">
<code class="language-php">$client->redirect('route_name', []);</code>
</pre>

<h4><a name="change-url">Change URL</a></h4>
In case you manually want to change the client's browser URL (no redirect), the client
interface provides you the <span class="code-hint">changeUrl(string $routeName, array $parameters = [], int $referenceType = UrlGeneratorInterface::ABSOLUTE_URL</span> method.

<pre class="imp-code code-white language-php">
<code class="language-php">$client->changeUrl('route_name', []);</code>
</pre>

<h4><a name="file-downloads">File downloads</a></h4>
The client interface provides methods for providing file downloads to the user.
Below you will find two different variants of file downloads.

<h5><a name="static-file-downloads">Static files</a></h5>
Static files mean already existing files that are publicly available by the webserver
and can be served for downloads. For this purpose, the client interface provides the method
<span class="code-hint">downloadFile</span>.

<pre class="imp-code code-white language-php">
<code class="language-php">$client->downloadFile('/path/to/the/file', 'name_of_the_file.txt');</code>
</pre>

The first parameter specifies the path together with the name of the file
that will be downloaded. The second, optional parameter, can be used if the 
provided download file will have another file name.

<h5><a name="dynamic-file-downloads">Dynamic files</a></h5>
Providing dynamically generated one time files like reports or statistics for download is a common task for web applications.
Hence the client interface provides a method with name <span class="code-hint">downloadRaw</span>.

<pre class="imp-code code-white language-php">
<code class="language-php">$client->downloadRaw('file.txt', 'Content of the file');</code>
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
<code class="language-php">$client->scrollTo(0, 0);</code>
</pre>

The code above will scroll the client's browser to the top left corner.
You may also leave out one coordinate and set it to null in order to do not
scrolling for that specific coordinate.

<h5><a name="scroll-up">Scroll up</a></h5>
The <span class="code-hint">scrollTop</span> is basically using the <span class="code-hint">scrollTo</span>
method but just sets the y-coordinate to 0.

<pre class="imp-code code-white language-php">
<code class="language-php">$client->scrollTop();</code>
</pre>

<h4><a name="execute-javascript">Execute javascript</a></h4>
You might face situations where your server needs to send javascript code
that must be executed in the browser. Luckily, that's where the
<span class="code-hint">executeJavascript</span> method is created for.

<pre class="imp-code code-white language-php">
<code class="language-php">$client->executeJavascript('(client) => console.log(client)');</code>
</pre>

It only takes one argument i.e. the javascript code defined as a callback. 
The callback itself takes exactly one argument, which is the Client object.

<h4><a name="log-messages">Log messages</a></h4>
You can also create log messages in the browser's console by using the
<span class="code-hint">log</span>.

<pre class="imp-code code-white language-php">
<code class="language-php">$client->log('this is a message', Impulse\ImpulseBundle\UI\Client\Commands\Log::LOG);</code>
</pre>

It offers the same log levels the console offers: log, info, debug, warn, error.

<pre class="imp-code code-white language-php">
<code class="language-php">$client->alert('this is a message', Log::LOG);</code>
</pre>

<h4><a name="alert-messages">Alert messages</a></h4>
You can also instruct the client to print browser alert messages to the user.

<pre class="imp-code code-white language-php">
<code class="language-php">$client->alert('this is an alert message');</code>
</pre>

<h4><a name="import-resources">Import resources</a></h4>
With the client interface you will also have the option to import
scripts or stylesheets on the fly.

<h5><a name="import-css">Import css</a></h5>
The code below shows how you can import a css file.

<pre class="imp-code code-white language-php">
<code class="language-php">$client->importCss('build/test.css', []);</code>
</pre>

The method itself offers three arguments. The first is the source of the file
which can be an absolute url or an URI that will be resolved by the Symfony's 
assets packages. The second parameter is optional and, if set, must be an associative
array that declares configurations like CORS.

The third parameter (default: true) indicates whether the source should be automatically
be resolved by the assets package or not.

It basically creates a link tag in the DOM.

<h5><a name="import-javascript">Import javascript</a></h5>

Analogous to css import, you can use the <span class="code-hint">importJavascript</span> method.
It offers the exact same method signature but instead will create a new script tag in the DOM.