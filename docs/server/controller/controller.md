---
layout: post
title: Start of Documentation
published: true
---
<h3 class="doc-title">Controllers</h3>

- [Introduction](#introduction)
- [Basics](#basics)
    - [Controller creation and binding](#controller-class)
    - [Access components](#access-components)
    - [Load views](#load-views)
    - [Inline views](#inline-views)
    - [Event listening](#listen-to-events)
- [Advanced topics](#advanced-topics)

<a name="introduction"></a>

Controllers in Impulse PHP Framework are not exactly the same as you might be used with other frameworks. In most other frameworks among other responsibilites, a controller is that instance that decides which view must be rendered. However, in impulse the exact opposite is the case that a view binds a concrete controller class as you have already explored in the **_Views_** section.

<h4><a id="basics">Basics</a></h4>

Controllers are in general created by the Symfonys dependency injection container. Thus, you have full autowiring support and can inject any services which you need for your controller.

<h5><a id="controller-class">Controller creation and binding</a></h5>

A minimalistic controller class is an empty class that extends the AbstractController class which is provided by the framework. The base class provides some useful functionalities that will be covered in a later section.

As previousely mentioned, a controller will be rarely called directly from the frameworks user. 

<pre class="imp-code code-white language-markup">
<code class="language-markup">&lt;window bind="App\Controller\AppController" /&gt;</code>
</pre>

The <span class="highlightText">bind</span> attribute defines which controller will be executed by the framework. The following controller is a minimal example. 

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
namespace App\Controller;
use Impulse\ImpulseBundle\Controller\AbstractController;
use Impulse\ImpulseBundle\Execution\Events\Event;

class AppController extends AbstractController
{
    public function afterCreate(Event $event)
    {
        parent::afterCreate($event);
    }
}</code>
</pre>

For doing initial tasks you can override the <span class="code-hint">afterCreate</span> method from the <span class="code-hint">AbstractController</span> or you can leave it out. The method will be called by the framework automativally once the view has been rendered and its components been created.

<h5><a name="access-components">Access components</a></h5>

The main purpose of controllers in Impulse is that they can directly interact with all components which have been created by views in an object orientated way. There are different strategies how these components can be accessed within a controller. 

<h6><a name="wiring-by-id">Wiring by id</a></h6>

One option is by wiring by convention by naming properties with the related component ids.

<pre class="imp-code code-white language-markup">
<code class="language-markup">&lt;window&gt;
    &lt;textbox id="tb" /&gt;
&lt;/window&gt;</code>
</pre>

We have created a textbox with the id <span class="highlightText">tb</span>. This component is - by convention - wireable for a controller and can be accessed via controller property.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
namespace App\Controller;
use Impulse\ImpulseBundle\Controller\AbstractController;
use Impulse\ImpulseBundle\Execution\Events\Event;
use Impulse\ImpulseBundle\UI\Components\Textbox;

class AppController extends AbstractController
{
    private ?Textbox $tb = null;

    public function afterCreate(Event $event)
    {
        parent::afterCreate($event);
        $this->tb->setValue('Hello world!');
    }
}</code>
</pre>
  
In the example above, once the controller was created, the afterCreate method will be called automatically (see chapter before) and the textbox value will be set to Hello World!.

Important note: A property of a Controller that is expected to be wired up by id with a component only works if the component has already been created previousely.

<h6><a name="get-component-method">getComponent method</a></h6>

If you do not want to have components injected into properties automatically and rather instead prefer to retrieve them explicitly. For this purpose, the AbstractController class provides the convenient method getComponentById to retrieve a component by it's id. 

The signature of this method is the following:

<pre class="imp-code  code-white language-php">
<code class="language-php">protected function getComponentById(PageContract $page, string $id): ?ComponentContract;</code>
</pre>

Important note: As of now this method does not consider the scope of the id. It is currently not possible to have a page with two identical ids in different scopes.

<h5><a id="load-views">Load views</a></h5>

The more complex the application gets the more it should be divided in smaller parts (single responsiblity principle). For exactly this purpose you can easily load views inside your controller. The following example shows how this works.

<pre class="imp-code code-white language-markup">
<code class=" language-markup">&lt;window id="wndMain" bind="App\Controller\AppController" /&gt;</code>
</pre>

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
namespace App\Controller;
use Impulse\ImpulseBundle\Controller\AbstractController;
use Impulse\ImpulseBundle\Execution\Events\Event;
use Impulse\ImpulseBundle\UI\Components\Window;

class AppController extends AbstractController
{
    private ?Window $wndMain = null;

    public function afterCreate(Event $event): void
    {
        $this->importView('importedView.imp', $this->wndMain);
    }
}</code>
</pre>
  
Line 13 is the important line. The <span class="highlightText">importView</span> method is called and takes two arguments. The first one is the view that will be imported and the second argument is the component that will be the direct parent of the root component of the imported view.

By default this will remove all childs of the given parent component internally. To append instead of removing all childs, you can set an optional third parameter with <i>true</i> as value.

<h5><a id="inline-views">Inline views</a></h5>

Though you can create complex component trees by creating them manually, sometimes it becomes more handy to create component objects on the fly by using inline views rather than real views. Inline views can be created and renderer within the controller. To achieve this, the <span class="code-hint">AbstractController</span> class offers a method called <span>createComponents</span>. A simple example could be creating a list based on array.

<pre class="imp-code code-white language-php">
<code class="language-php"><?php
namespace App\Controller;
use Impulse\ImpulseBundle\Controller\AbstractController;
use Impulse\ImpulseBundle\Execution\Events\Event;
use Impulse\ImpulseBundle\UI\Components\Div;

class AppController extends AbstractController
{
    private ?Div $container = null;

    public function handleEvent(Event $event)
    {
        $values = range('A', 'Z');
        $this->createComponents('&lt;ul&gt;
            &#123;% for value in values %}
                &lt;li&gt;&#123;&#123; value }}&lt;/li&gt;
            &#123;% endfor %};
        &lt;/ul&gt;', $this->container, compact('values'));
    }
}</code>
</pre>

Like in real twig template you can also use the twig functionalities in inline views as well.

<h4><a id="event-listening">Event listening</a></h4>

How components are registered as event listeners can be read in the Events documentation.
