<h3 class="doc-title">Volatile components</h3>

Assume you would like to have a component that is not stored in the users session (server side) and instead just resides at the client side. The Notification component for example implements this interface and is marked as volatile and is thus not managed by the server.

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
  <pre class="code-white language-php">
	<code class="imp-code language-php"><?php
	namespace Impulse\ImpulseBundle\UI\Components;
	use Impulse\ImpulseBundle\Components\VolatileInterface;

    class Notification extends AbstractComponent implements VolatileInterface
	{
    	// ... properties and methods
    }</code>
  </pre>
</div>

In future there will be native components implemented that marked as volatile to optimize processing speed & memory consumption since they don't need to be managed by the server.
