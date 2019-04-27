---
layout: post
title: Start of Documentation
---

<a name="listeners"></a>
### Controller listeners

Just defining the controller as itself for initial tasks is by far not the only feature. As you have maybe noticed, controllers can also work as event listeners as well. Consider the following example.

<pre class="line-numbers language-markup">
<code class="language-markup">&lt;impulse&gt;
    &lt;window apply="App\Controller\AppController"&gt;
        &lt;textbox id="tbName" /&gt;
        &lt;button id="btnGreet" /&gt;
        &lt;label id="lbGreet" /&gt;
    &lt;/window&gt;
&lt;/impulse&gt;</code>
</pre>

The controller contains an event listener method.

<pre class="line-numbers language-php">
<code class="language-php">
<?php
namespace App\Controller;
use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;
use Impulse\Bundles\ImpulseBundle\Execution\Events\Event;
use Impulse\Bundles\ImpulseBundle\Controller\Annotations\Listen;
use Impulse\Bundles\ImpulseBundle\UI\Components\Label;
use Impulse\Bundles\ImpulseBundle\UI\Components\Textbox;

class AppController extends AbstractController
{
    /** @var Textbox */ private $tbName;
    /** @var Label */   private $lbGreet;

    /**
     * @Listen(component="btnGreet", event="click")
     */
    public function onClick(Event $event)
    {
        $greeting = $this->tbName->getValue();
        $this->lbGreet->setValue($greeting);
    }
}</code>
</pre>

Internally, the framework scans controllers for methods which are annotated by <i>@Listen</i>. This annotation creates a connection between a client interaction (in this case a click event) of a component with the controller. Whenever the button <b>btnGreet</b> is clicked in the browser, the AJAX engine sends a request to execute the <i>onClick</i> method in the AppController.
