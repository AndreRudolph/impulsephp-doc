<h3 class="doc-title">Serialization</h3>

- [Introduction](#introduction)
- [Exclude properties](#exclude-properties)
- [Component references](#component-references)
- [EventListener references](#eventlistener-references)

<h5><a id="introduction">Introduction</a></h5>

Like components, controllers will also be serialized and stored in the users session when they are registered as event listeners. The concept of serialization is to have a string representation of an object that stores its states that must be restored once the serialized object will be deserialized. 

In this documentation page we will cover the essentials of controller serialization and how you can customize the serialization with your needs.

<h5><a id="exclude-properties">Exclude properties</a></h5>

It is likely that you have to exclude properties manually from serialization. Assuming that you have injected a service that is connected to the database or any other resource that is not serializable. In order to exclude these properties from serialization, you can simply annotate them with the **Transient**__ annotation.

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

When trying to serialize the controller, all properties annotated with **_Transient_** are automatically excluded.

<h5><a id="component-references">Component references</a></h5>

<h5><a id="eventlistener-references">Event listener references</a></h5>





























