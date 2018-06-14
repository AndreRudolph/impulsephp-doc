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
<span class="token delimiter">&lt;?php</span>
<span class="token keyword">namespace</span> <span class="token package">Impulse<span class="token punctuation">\</span>Bundles<span class="token punctuation">\</span>DevBundle<span class="token punctuation">\</span>Controller</span><span class="token punctuation">;</span><span class="token keyword">use</span> <span class="token package">Impulse<span class="token punctuation">\</span>Bundles<span class="token punctuation">\</span>ImpulseBundle<span class="token punctuation">\</span>Controller<span class="token punctuation">\</span>AbstractController</span><span class="token punctuation">;</span>
<span class="token keyword">use</span> <span class="token package">Impulse<span class="token punctuation">\</span>Bundles<span class="token punctuation">\</span>ImpulseBundle<span class="token punctuation">\</span>Execution<span class="token punctuation">\</span>Events<span class="token punctuation">\</span>Event</span><span class="token punctuation">;</span>
<span class="token keyword">use</span> <span class="token package">Impulse<span class="token punctuation">\</span>Bundles<span class="token punctuation">\</span>ImpulseBundle<span class="token punctuation">\</span>Controller<span class="token punctuation">\</span>Annotations<span class="token punctuation">\</span>Listen</span><span class="token punctuation">;</span>
<span class="token keyword">use</span> <span class="token package">Impulse<span class="token punctuation">\</span>Bundles<span class="token punctuation">\</span>ImpulseBundle<span class="token punctuation">\</span>UI<span class="token punctuation">\</span>Components<span class="token punctuation">\</span>Label</span><span class="token punctuation">;</span>
<span class="token keyword">class</span> <span class="token class-name">HomeController</span> <span class="token keyword">extends</span> <span class="token class-name">AbstractController</span>
<span class="token punctuation">{</span><span class="token comment" spellcheck="true">/** @var Label */</span> <span class="token keyword">private</span> <span class="token variable">$lbClicked</span><span class="token punctuation">;</span><span class="token keyword">public</span> <span class="token keyword">function</span> <span class="token function">handleEvent</span><span class="token punctuation">(</span>Event <span class="token variable">$event</span><span class="token punctuation">)</span>
    <span class="token punctuation">{</span>
        <span class="token comment" spellcheck="true">// TODO: Implement handleEvent() method.</span>
    <span class="token punctuation">}</span>

    <span class="token comment" spellcheck="true">/**
     * @Listen(component="btnClick", event="click")
     */</span>
    <span class="token keyword">public</span> <span class="token keyword">function</span> <span class="token function">onClick</span><span class="token punctuation">(</span>Event <span class="token variable">$event</span><span class="token punctuation">)</span>
    <span class="token punctuation">{</span>
        <span class="token variable">$this</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token property">lbClicked</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token function">setValue</span><span class="token punctuation">(</span><span class="token string">'Clicked!'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
<span class="token punctuation">}</span><span aria-hidden="true" class="line-numbers-rows"><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span><span></span></span> 
    </code>
</pre>



```php


    <?php

    namespace Impulse\Bundles\ImpulseBundle\Events;


    /**
     * Class Events
     *
     * @package Impulse\Bundles\ImpulseBundle\Events
     * @author  Andre Rudolph
     * @version 1.0
     */
    class Events {

        const CLICK = 'click';

        const HOVER = 'mouseenter';

        const SCHEDULED = 'scheduled';

        const INTERNAL = 'internal';
    }
```
    
    
