---
layout: post
title: Start of Documentation
---


# Controllers

- [Introduction](#introduction)
- [Basic Controllers](#basic-controllers)
    - [Defining Controllers](#defining-controllers)
- [Controllers as event listener] (#controller-eventlistener)
    - [Defining an event listener] (#defining-eventlistener)

<a name="introduction"></a>
## Introduction

A controller is the main component that is responsible for handling (and thus managing) user interactions. Unlike other frameworks, a controller in the Impulse PHP Framework itself does not directly return views. Instead, a view template specifies the related controller that will be rendered. 

<a name="basis-controller">
## Basic controllers
    
### Defining controllers
Defining a controller is not very complicated. The following example is the bare minimum that composes a controller.  
  
<pre class="line-numbers language-php">
<code class="language-php">
<?php
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
        // TODO: Implement handleEvent() method.
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


Test

- a
- b
- c

> {tip} Looking for more information?

<b>Test</b>

<button class="a-button">Das ist ein Testbutton!</button>

<div id="UGQe2$UR60i" class="documentationHint" style=""><span id="UGQe2$IC4mu" class="label" style="">This is an info box! Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.</span></div>


