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
    &lt;window apply="App\Controller\AppController" /&gt;
&lt;/impulse&gt;</code>
</pre>

The <span class="highlightText">apply</span> attribute defines which controller will be directly connected to the view. The following controller example is the bare minimum.  
  
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
You may automatically inject components, which are placed a view template, within your controller. Wiring components into controllers are done by naming conventions. Whenever you define an ID to your component, the component is injectable into your controller.

For instance, consider the folling view:

<pre class="line-numbers language-markup">
<code class="language-markup">&lt;impulse&gt;
    &lt;window id="wndApp" apply="App\Controller\AppController" /&gt;
&lt;/impulse&gt;</code>
</pre>

It's the same example with an exception: the window component has now an ID. Thus, you can connect the window component to your AppController:

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

<div class="alert alert-warning" role="alert">
  At the moment, only wiring with IDs are supported. In future, there might be support for CSS based selectors.
</div>

