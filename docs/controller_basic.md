---
layout: post
title: Start of Documentation
---

<a name="introduction"></a>
# Introduction

A controller is the main component that is responsible for handling (and thus managing) user interactions. Unlike other frameworks, a controller in the Impulse PHP Framework itself does not directly return views. Instead, a view template specifies the related controller that will be rendered. 

<a name="basis-controller">
# Basic controllers
    
### Defining controllers
Defining a controller is not very complicated. A controller is never called directly from the frameworks user. Instead a controller will be bound to a certain template.

<pre class="line-numbers language-markup">
<code class="language-markup">&lt;impulse&gt;
    &lt;window apply="App\Controller\AppController" /&gt;
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

This example controller above would do nothing. For doing initial tasks you may override the handleEvent method from the AbstractController. The <span class="highlightText">handleEvent</span> represents the entry point of the controller and will  will be called automatically if the controller is bound to a view template.
