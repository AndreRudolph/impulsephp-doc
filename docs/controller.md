---
layout: post
title: Start of Documentation
---


# Controllers

- [Introduction](#introduction)
- [Basic Controllers](#basic-controllers)
    - [Defining Controllers](#defining-controllers)
- [Controllers as event listener](#controller-eventlistener)
    - [Defining an event listener](#defining-an-event-listener)

<a name="introduction"></a>
## Introduction

A controller is the main component that is responsible for handling (and thus managing) user interactions. Unlike other frameworks, a controller in the Impulse PHP Framework itself does not directly return views. Instead, a view template specifies the related controller that will be rendered. 

<a name="basis-controller">
## Basic controllers
    
### Defining controllers
Defining a controller is not very complicated. The following example is the bare minimum that composes a controller.  
  
<pre class="line-numbers language-php">
<code class="language-php"><?php
namespace App\Controller;
use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;
use Impulse\Bundles\ImpulseBundle\Execution\Events\Event;

class AppController extends AbstractController
{
    public function handleEvent(Event $event)
    {
        // app specific controller logic
    }
}</code>
</pre>

The handleEvent method is the only required method and represents the entry point of the controller. This method will be called automatically if the controller is bound to a view template.

<pre class="line-numbers language-markup">
<code class="language-markup">&lt;window apply="App\Controller\AppController" /&gt;</code>
</pre>

<a name="controller-eventlistener"></a>
## Controllers as event listener

### Defining an event listener
Just defining a controller is by far not the only supported feature. As you maybe noticed, controllers can also work as event listeners as well. Consider the following example.

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
    /** @var Label */ private $lbGreet;

    public function handleEvent(Event $event)
    {
        // app specific controller logic
    }

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

The controller has been extended by two new attributes and an onClick method. The name of the attributes correlate with their id in the view template. Thus, the framework can automatically inject these component objects to the controller. The onClick method is annotated with a @Listen annotation. This is a marker for the framework to automatically register an event listener. 

The annotation consists of two parameters. The component parameter requires (at the moment) one id of a component for which the event listener will be registered. The second parameter defines the event type that must be triggered on the client side to fire the execution of the event listener.

