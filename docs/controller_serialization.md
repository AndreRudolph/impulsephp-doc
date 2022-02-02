<h3 class="doc-title">Serialization</h3>

- [Introduction](#introduction)
- [Exclude properties](#exclude-properties)
- [Component references](#component-references)
- [EventListener references](#eventlistener-references)

<h5><a id="introduction">Introduction</a></h5>

<a href="https://en.wikipedia.org/wiki/Serialization" target="_blank">Serialization</a> is a process of converting an object into a string based representation that can later be restored to its original state (object) if needed. Like components, controllers registered as event listeners will also be serialized once the request will be finished. The framework retrieves all properties and values by reflection.

Once a controller will be reactivated in a subsequent request, it will be deserialized and brought back to its previous state. If, during the event processing, the controller changes it state, it will be then serialized again with its new state for later requests.

<h5><a id="exclude-properties">Exclude properties</a></h5>

Typically values containing resources like database connections or file handles can not be serialized and every attempt to serialize them will lead to an error. It is likely that you need services inside your controllers that either have explicit or implicit access to a database or filesystem. 

To exclude these properties from the serialization process you can simply annotate them with <span class="code-hint">#[Transient]</span> attribute.

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

Please note that transient properties will not be rewired once the Controller will be reactivated unless they will be wired by the dependency injection container.

<h5><a id="component-references">Component references</a></h5>

You might face the issue that you need to keep component references in properties of the controller to access them once the event listener will be restored. In general, component objects are stored outside the scope of
the controller. However, if you need to store a list of component references, you need to have a property with type ComponentList.

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
