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

The controller has been extended by two new attributes and an <span class="highlightText">onClick</span> method. The name of the attributes correlate with their id in the view template. Thus, the framework can automatically inject these component objects to the controller. The <span class="highlightText">onClick</span> method is annotated with a <span class="highlightText">@Listen</span> annotation. This is a marker for the framework to automatically register an event listener. 

The annotation consists of two parameters. The component parameter requires one component id or a comma separated list of components ids for which the event listener will be registered. The second parameter defines the event type that must be triggered on the client side to fire the execution of the event listener.
