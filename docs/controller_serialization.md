<h3 class="doc-title">Serialization</h3>

- [Introduction](#introduction)
- [Exclude properties](#exclude-properties)
- [Component references](#component-references)
- [EventListener references](#eventlistener-references)

<h5><a id="introduction">Introduction</a></h5>

Like components, controllers will also be serialized and stored in the users session when they are registered as event listeners. The concept of serialization is to have a string representation of an object that stores its states that must be restored once the serialized object will be deserialized.

Once an event listener (controller) will be reactivated (deserialized), values that have been stored in serialized format will be restored. After the request has been processed, the reactivated event listener will be stored back in session with its current state.

In this documentation page we will cover the essentials of controller serialization and how you can customize the serialization for your needs.

<h5><a id="exclude-properties">Exclude properties</a></h5>

It is likely that you have to exclude properties manually from serialization. Assuming that you have injected a service that is connected to the database or any other resource that is not serializable. In order to exclude these properties from serialization, you can simply annotate them with the **_Transient_** annotation.

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
            <div class="button red"></div>
          	<div class="button yellow"></div>
          	<div class="button green"></div>
        </div>
    </div>
  </div>
  <pre class="code-white line-numbers language-php">
  	<code class="imp-code language-php"><?php
	namespace App\Controller;
	use Impulse\ImpulseBundle\Controller\AbstractController;
	use Impulse\ImpulseBundle\Controller\Annotations\Transient;
    use Doctrine\ORM\EntityManagerInterface;

	class AppController extends AbstractController
	{
    	#[Transient]
		private EntityManagerInterface $em;
        
        public function __construct(EntityManagerInterface $em)
        {
        	$this->em = $em;
        }

        // other methods
	}</code>
  </pre>
</div>

When trying to serialize the controller, all properties annotated with **_Transient_** will automatically be excluded.

<h5><a id="component-references">Component references</a></h5>

You might face the issue that you need to keep component references in properties of the controller to access them once the event listener will be restored. In general, component objects in properties are correctly stored and referenced automatically. You can bypass this by declaring it transient as explained above. However, if you need to store a list of component references, you need to have a property with type ComponentList. 

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
            <div class="button red"></div>
          	<div class="button yellow"></div>
          	<div class="button green"></div>
        </div>
    </div>
  </div>
  <pre class="code-white line-numbers language-php">
  	<code class="imp-code language-php"><?php
	namespace App\Controller;
	use Impulse\ImpulseBundle\Controller\AbstractController;
    use Impulse\ImpulseBundle\Execution\ComponentList;
    use Impulse\ImpulseBundle\UI\Components\Button;
    use Impulse\ImpulseBundle\UI\Components\Label;

	class AppController extends AbstractController
	{
    	private Button $btn;
        private Label $lb;
    	private ?ComponentList $myComponentList;
        
        public function __construct()
        {
        	super::__construct();
        }
        
        public function afterCreate()
        {
        	parent::afterCreate();
            $this->myComponentList = new ComponentList($button);
            $this->myComponentList->add($lb);
        }

        // other methods
	}</code>
  </pre>
</div>

<h5><a id="eventlistener-references">Event listener references</a></h5>

In some scenarios you need to execute logic that is placed in another controller (like showing a success notification) and not in your current controller object. However, like component references you can also include event listener references in the serialization by having properties referencing to objects of class EventListenerReference.
