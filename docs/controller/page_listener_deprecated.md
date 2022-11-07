<h1 class="doc-title">Page listener</h1>

- [Introduction](#introduction)
- [Register as page listener](#register-page-listener)
- [Broadcast event to subscribers](#broadcast-event)
- [Unsubscribe page listener](#unsubscribe-page-listener)

<h4><a id="#introduction">Introduction</a></h4>

Aswell as controller can be registered as an event listener to several events that a component can trigger, controller might also register themselfes as so called global page listener. The intention behind global page listener is provide a mechanism to broadcast the result of an event (e.g. a record was updated) to all other existing of controller the same page that have been subscribed to that event.

Consider you have a list of user records and each of them can be changed within a modal window. When the user saves the changes, the list Controller might need to be notified from the modal window controller to refresh the list by updating the users data in the list.

<h4><a id="#register-page-listener">Register as page listener</a></h4>

A controller can be easily registered as a global page listener by calling the subscribe method of the page object.

<div class="code-header">
	<div class="container-fluid">
		<div class="row">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
    </div>
</div>
<pre class="code-white  language-php">
	<code class="imp-code language-php"><?php
	namespace App\Controller;
    use Impulse\ImpulseBundle\Controller\AbstractController;
    use Impulse\ImpulseBundle\Execution\Events\Event;
    use Impulse\ImpulseBundle\UI\Components\Window;

    class UserListController extends AbstractController
    {
    	private ?Window $wndUserList = null;
    
    	public function afterCreate(Event $event): void
    	{
        	parent::afterCreate($event);
        	$this->createList();

        	$event->getPage()->subscribe('UserSaved', $this, 'onUserSave');
    	}
        
        public function onUserSave(): void
        {
        	$this->createList();
        }
        
        private function createList(): void
        {
        	// creates the list of user records
        }
	}</code>
</pre>

A controller can serve as a global page listener for different events.

<h4><a id="#broadcast-event">Broadcast event to subscribers</a></h4>

As previousely mentioned, a global page event can be triggerd by any controller. Let's follow the example above and have a UserDetailsController that broadcasts the event of a saved user.

<div class="code-header">
	<div class="container-fluid">
		<div class="row">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
    </div>
</div>
<pre class="code-white  language-php">
	<code class="imp-code language-php"><?php
	namespace App\Controller;
    use Impulse\ImpulseBundle\Controller\AbstractController;
    use Impulse\ImpulseBundle\Execution\Events\Event;

    class UserDetailsController extends AbstractController
    {       
    	#[Listen(event: Events::CLICK, component: 'btnSaveUser')]
        public function onUserSave(ClickEvent $event): void
        {
        	// ... validation and save logic before
            
        	$event->getPage()->sendEvent('UserSaved');
        }
	}</code>
</pre>

You may also pass an array of parameters to the sendEvent method. An instance of the class PageListenerEvent will be automatically be passed to every subscribed page listener. It provides a getter method to retrieved the previousely passed arguments.

<h4><a id="#unsubscribe-page-listener">Unsubscribe page listener</a></h4>

When a global page listener is no longer referenced from any component and thus be removed from the page, the controller will be automatically unsubscribed as a global page listener. 

However, when you want or need to unsubscribe the controller manually beforehand, then you can use the unsubscribe method of the page object. Let's take as an example that you want to remove the subscription once the UserListController has been broadcasted in the first place. 

<div class="code-header">
	<div class="container-fluid">
		<div class="row">
          <div class="button red"></div>
          <div class="button yellow"></div>
          <div class="button green"></div>
        </div>
    </div>
</div>
<pre class="code-white  language-php">
	<code class="imp-code language-php"><?php
	namespace App\Controller;
    use Impulse\ImpulseBundle\Controller\AbstractController;
    use Impulse\ImpulseBundle\Execution\Events\Event;
    use Impulse\ImpulseBundle\UI\Components\Window;

    class UserListController extends AbstractController
    {
    	private ?Window $wndUserList = null;
    
    	public function afterCreate(Event $event): void
    	{
        	parent::afterCreate($event);
        	$this->createList();
        	$event->getPage()->subscribe('UserSaved', $this, 'onUserSave');
    	}
        
        public function onUserSave(): void
        {
        	$this->wndUserList->getPage()->unsubscribe('UserSaved', $this, 'onUserSave');
        	$this->createList();
        }
        
        private function createList(): void
        {
        	// creates the list of user records
        }
	}</code>
</pre>
