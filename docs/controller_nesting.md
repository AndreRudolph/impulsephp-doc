---
layout: post
title: Start of Documentation
---

<a name="nesting"></a>

### Nested controllers
Thanks to nesting views you may nest controllers as well. Consider a web page consisting of different page sections like navigation, content, footer, etc. The bad approach would be that you would create one controller for the whole page. As a good software developer you try to separate concerns as much as possible and reasonable. That's why the framework offers a feature that is called <b>controller nesting</b>.

With controller nesting you can create a tree of controllers. Like view nesting, this can be achieved by nesting views and applying controllers to them (if needed). Consider the following example for a simple navigation (wihtout functions):

<ul class="nav nav-tabs" id="myTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="home-tab" data-toggle="tab" href="#home" role="tab" aria-controls="home" aria-selected="true">app.imp</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="profile-tab" data-toggle="tab" href="#profile" role="tab" aria-controls="profile" aria-selected="false">AppController.php</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="contact-tab" data-toggle="tab" href="#contact" role="tab" aria-controls="contact" aria-selected="false">navigation.imp</a>
  </li>
    <li class="nav-item">
    <a class="nav-link" id="navController-tab" data-toggle="tab" href="#navController" role="tab" aria-controls="navController" aria-selected="false">NavigationController.php</a>
  </li>
</ul>
<div class="tab-content" id="myTabContent">
  <div class="tab-pane fade show active" id="home" role="tabpanel" aria-labelledby="home-tab">
    <pre class="line-numbers language-markup">
      <code class="language-markup">&lt;impulse&gt;
        &lt;window id="wndApp" apply="App\Controller\AppController"&gt;
            &lt;import src="navigation.imp" /&gt;
            &lt;div id="content" /&gt;
        &lt;/window&gt;
        &lt;/impulsew&gt;
      </code>
    </pre>
  </div>
  <div class="tab-pane fade" id="profile" role="tabpanel" aria-labelledby="profile-tab">
    <pre class="line-numbers language-php">
      <code class="language-php"><?php
      namespace App\Controller;
      use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;

      class AppController extends AbstractController
      {

      }</code>
    </pre>  
  </div>
  <div class="tab-pane fade" id="contact" role="tabpanel" aria-labelledby="contact-tab">
    <pre class="line-numbers language-markup">
      <code class="language-markup">&lt;impulse&gt;
          &lt;window id="wndNavigation" apply="App\Controller\NavigationController"&gt;
              &lt;hbox&gt;
                  &lt;label&gt;Home&lt;/label&gt;
                  &lt;label&gt;News&lt;/label&gt;
              &lt;/hbox&gt;
          &lt;/window&gt;
        &lt;/impulse&gt;
      </code>
    </pre>
  </div>
  <div class="tab-pane fade" id="navController" role="tabpanel" aria-labelledby="navController-tab">
    <pre class="line-numbers language-php">
      <code class="language-php"><?php
      namespace App\Controller;
      use Impulse\ImpulseBundle\UI\Components\Div;
      use Impulse\Bundles\ImpulseBundle\Controller\AbstractController;

      class NavigationController extends AbstractController
      {
          /** @var Div */
          private $content;   

          public function handleEvent()
          {
              parent::handleEvent();
              $labelContent = new Label();
              $labelContent->setValue('Content is here.');
              $this->content->addChild($content);
          }
      }</code>
    </pre>
  </div>
</div>

### Importing views via controller
The above example is not the only possibility to nest views. The AbstractController provides a method <i>importView</i> which is also able to import views. This is more sophisticated since you have full control of it. You may add your own logic if and when you want to import a specific view to your page.

<h5>importView - Parameters</h5>
<table class="table table-hover">
    <tbody>
        <tr class="d-flex">
          <td class="col-1">view</td>
          <td class="col-1">string</td>
          <td class="col-4">Name of the template that shall be imported</td>
          <td class="col-2"><b>required</b></td>
        </tr>
    </tbody>
</table>
