---
layout: post
title: Start of Documentation
---

Test

- a
- b
- c

> {tip} Looking for more information?

<b>Test</b>

<button class="a-button">Das ist ein Testbutton!</button>

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
