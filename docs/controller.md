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

Instead of defining all of your request handling logic as Closures in route files, you may wish to organize this behavior using Controller classes. Controllers can group related request handling logic into a single class. Controllers are stored in the `app/Http/Controllers` directory.



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
