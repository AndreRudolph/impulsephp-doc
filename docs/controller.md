---
layout: post
title: Start of Documentation
---


# Controllers

- [Introduction](#introduction)
- [Basic Controllers](#basic-controllers)
    - [Defining Controllers](#defining-controllers)

<a name="introduction"></a>
## Introduction

A controller is the main component that is responsible for handling (and thus managing) user interactions. Unlike other frameworks, a controller in the Impulse PHP Framework itself does not directly return views. Instead, a view template specifies the related controller that will be rendered. 

<a name="basis-controller">
## Basic controllers
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

<pre class="line-numbers language-xml">
<code class="language-xml">&lt;window apply="App\Controller\AppController" /&gt;</code>
</pre>

Test

- a
- b
- c

> {tip} Looking for more information?

<b>Test</b>

<button class="a-button">Das ist ein Testbutton!</button>

<div id="UGQe2$UR60i" class="documentationHint" style=""><span id="UGQe2$IC4mu" class="label" style="">This is an info box! Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.</span></div>

<pre class="line-numbers language-php">
<code class="language-php">
<?php
namespace Impulse\Bundles\DevBundle\Controller;
use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;
use Impulse\Bundles\ImpulseBundle\Execution\Events\Event;
use Impulse\Bundles\ImpulseBundle\Controller\Annotations\Listen;
use Impulse\Bundles\ImpulseBundle\UI\Components\Label;

class HomeController extends AbstractController
{

    /** @var Label */ private $lbClicked;

    public function handleEvent(Event $event)
    {
        // TODO: Implement handleEvent() method.
    }

    /**
     * @Listen(component="btnClick", event="click")
     */
    public function onClick(Event $event)
    {
        $this->lbClicked->setValue('Clicked!');
    }
}</code>
</pre>
