---
layout: post
title: Start of Documentation
---

<a name="nesting"></a>

### Nested controllers
Thanks to nesting views you may nest controllers as well. Consider a web page consisting of different page sections like navigation, content, footer, etc. The bad approach would be that you would create one controller for the whole page. As a good software developer you try to separate concerns as much as possible and reasonable. That's why the framework offers a feature that is called <b>controller nesting</b>.

With controller nesting you can create a tree of controllers. Like view nesting, this can be achieved by nesting views and applying controllers to them (if needed). Consider the following example for a simple navigation:

<pre class="line-numbers language-markup">
<code class="language-markup"><impulse>
    <window id="wndApp" apply="App\Controller\AppController">
    
    </window>
</impulse>
</code>
</pre>
