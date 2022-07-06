<h3 class="doc-title">Events</h3>

- [Introduction](#introduction)
- [Controllers](#controllers)
- [Components](#components)
- [Global listener](#global-listener)
	- [Subscribe](#subscribe-listener)
    - [Broadcast event](#broadcast-event)
    - [Unsubscribe](#unsubscribe-listener)


<h4><a id="introduction">Introduction</a></h4>
Event based architectures are a good and common practice to separate concerns and responsibilities between event emitter and event listener. In the previous documentations of controllers and components you have learned how event listeners can be easily added to your application.

However, this article explains events more in depth and will also cover the topic of page global event listener.

<h4><a id="controllers">Controllers</a></h4>
There are basically three different options to register a controller as event listener. In practice you might use a combination of all of them since each of them has pros / cons and limitations.

<h6>Attributes approach</h6>
A very simple but yet limited approach is by using attributes.

Code here

The drawback of this approach is that you can only pass ids of components since this feature does not support any kind of dynamic declarations (such as css 3 selectors) for now. 

<h6>Manual approach</h6>
The second approach might be the most reliable approach since you can also register event listeners on newly created components or such of which you don't have any internal id known or set.

Code here

<h6>Template approach</h6>
With the last option you can easily register the controller as listener to component events inside a template. This only works if the template has a controller bounded.

Code here

<h4><a id="components">Components</a></h4>

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
