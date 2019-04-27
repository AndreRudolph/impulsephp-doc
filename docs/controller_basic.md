---
layout: post
title: Start of Documentation
---

<a name="introduction"></a>
# Introduction

A controller is the main component that is responsible for handling (and thus managing) user interactions. Unlike other frameworks, a controller in the Impulse PHP Framework itself does not directly return views. Instead, a view template specifies the related controller that will be rendered. 
    
### Defining controllers
Defining a controller is not complicated. Unlike other frameworks, a controller will be bound to a certain template.

<pre class="line-numbers language-markup">
<code class="language-markup">&lt;impulse&gt;
    &lt;window id="wndApp" apply="App\Controller\AppController" /&gt;
&lt;/impulse&gt;</code>
</pre>

The <span class="highlightText">apply</span> attribute defines which controller will be executed by the framework. The following controller example is the bare minimum.  
  
<pre class="line-numbers language-php">
<code class="language-php"><?php
namespace App\Controller;
use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;

class AppController extends AbstractController
{
    
}</code>
</pre>

This example controller above would do nothing. For doing initial tasks you may override the handleEvent method from the AbstractController. The <span class="highlightText">handleEvent</span> represents the entry point of the controller and will  will be called automatically if the controller is bound to a view template. This gets invoked after the controller has been created. 

### Wiring components
In the following code listing, a new Label with a greeting message will be appended to the wiried window component.

<pre class="line-numbers language-php">
<code class="language-php"><?php
namespace App\Controller;
use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;

class AppController extends AbstractController
{
    private $wndApp;

    public function handleEvent()
    {
        $lbGreet = new Label();
        $lbGreet->setValue('Welcome stranger!');
        $this->wndApp->addChild($lbGreet);
    }
}</code>
</pre>
