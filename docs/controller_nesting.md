---
layout: post
title: Start of Documentation
---

<a name="nesting"></a>

### Nested controllers
Another main feature is view and therefor controller nesting. See more for view nesting in the views documentation. This section will only cover controller nesting. This can be achieved by importing a template within the executed controllers method. Below is an example of controller nesting.

<pre class="line-numbers language-markup">
<code class="language-markup">&lt;impulse&gt;
    &lt;window id="wndMain" apply="App\Controller\AppController" /&gt;
&lt;/impulse&gt;</code>
</pre>

<pre class="line-numbers language-php">
<code class="language-php"><?php
namespace App\Controller;
use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;
use Impulse\Bundles\ImpulseBundle\Execution\Events\Event;
use Impulse\Bundles\ImpulseBundle\UI\Components\Window;

class AppController extends AbstractController
{
    /** @var Window */ private $wndMain;

    public function handleEvent(Event $event)
    {
        $this->importView('importedView.imp', $this->wndMain);
    }
}</code>
</pre>

Line 13 is the important line. The <span class="highlightText">importView</span> method is called and takes two arguments. The first one is the view that will be imported and the second argument is the component that will be the direct parent of the root component of the imported view.

By default this will remove all childs of the given parent component internally. To append instead of removing all childs, you can set an optional third parameter with <i>true</i> as value.
