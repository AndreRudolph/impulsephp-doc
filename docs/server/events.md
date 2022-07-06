<h3 class="doc-title">Events</h3>

- [Introduction](#introduction)
- [Controller listener](#controller-listener)
- [Component listener](#component-listener)
- [Global listener](#global-listener)
	- [Subscribe](#subscribe-listener)
    - [Broadcast event](#broadcast-event)
    - [Unsubscribe](#unsubscribe-listener)


<h4><a id="introduction">Introduction</a></h4>
Event based architectures are a good and common practice to separate concerns and responsibilities between event emitter and event listener. In the previous documentations of controllers and components you have learned how event listeners can be easily added to your application.

However, this article explains events more in depth and will also cover the topic of page global event listener.

<h4><a id="controller-listener">Controller listener</a></h4>

<h4><a id="component-listener">Component listener</a></h4>

<h4><a id="global-listener">Global listener</a></h4>

Very complex web applications may consist of many different, independent sections that are losely connected to each other conceptually. Sometimes, when something happens in one section, e.g. a record will be updated, other sections might be interested in listening to this event and update themselfes accordingly. 

This is a use case for global listener. A controller or component can subscribe themselfes as a listener to one or more global events and are automatically invoked once one of these events are triggered. 

The following examples are component based but controllers can be registered the same way as global listener. The subscribe, unsubscribe and broadcast are handled by the Page instance.

For this article we assume a simple application with a left sided user list and a right sided user details view. Whenever you select an user, the user details will be populated based on the user. Once you saved the user by clicking on the save button, the changes will be persisted and the user list will be notified about the change.

<h5><a id="subscribe-listener">Subscribe</a></h5>

According to the agreed example above we will have a UserList component that subscribes itself to the user change event. 

<pre class="code-white language-php">
	<code class="imp-code language-php"><?php

	class UserList extends Ul
	{
        public function afterCreateChildren(): void
        {
        	parent::afterCreateChildren();
        	$this->getPage()->subscribe('userChanged', $this, 'onUserChanged');
        }
        
        public function onUserChanged(PageContract $page, User $user): void
        {
        	// update the list
        }
	}</code>
</pre>

The first parameter is reserved for the Page object. All subsequent parameters are coming from the parameters passed to the broadcast that will be covered in the next chapter. 

<h5><a id="broadcast-event">Broadcast event</a></h5>

A global event can be broadcast from anywhere inside a controller or component by calling the <span class="code-hint">broadcast</span> method of the page object. The following simplified component represents the UserDetails component that broadcasts the <span class="code-hint">userChanged</span> event once the save button has been clicked.

<pre class="code-white language-php">
	<code class="imp-code language-php"><?php

	class UserDetails extends Div
	{
        private ?Button $btnSave = null;
        
        public function afterCreateChildren(): void
        {
        	parent::afterCreateChildren();
            $this->btnSave->addEventListener(Events::CLICK, $this, 'onUserSave');
        }
        
        public function onUserSave(ClickEvent $event): void
        {
        	// $user = ...
            $this->getPage()->sendEvent('userChanged', $user);
        }
	}</code>
</pre>

<h5><a id="unsubscribe-listener">Unsubscribe listener</a></h5>

When a global page listener is no longer referenced from any component and thus be removed from the page, the controller will be automatically unsubscribed as a global page listener. 

However, when you want or need to unsubscribe the controller / component manually beforehand, then you can use the unsubscribe method of the page object. Let's take as an example that you want to remove the subscription once the UserList component has been detached.

<pre class="code-white language-php">
	<code class="imp-code language-php"><?php

	class UserList extends Ul
	{
        public function afterCreateChildren(): void
        {
        	parent::afterCreateChildren();
        	$this->addEventListener(Events::DETACH, $this, 'onDetach');
        }
        
        public function onDetach(): void
        {
        	$this->getPage()->unsubscribe('userChanged', $this, 'onUserChanged');
        }
	}</code>
</pre>
