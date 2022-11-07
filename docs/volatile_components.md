<h3 class="doc-title">Volatile components</h3>

Assume you would like to have a component that is not stored in the users session (server side) and instead just resides at the client side. The Notification component for example implements this interface and is marked as volatile and is thus not managed by the server.

<pre class="imp-code ode-white language-php">
<code class="language-php"><?php
namespace Impulse\ImpulseBundle\UI\Components;
use Impulse\ImpulseBundle\Components\VolatileInterface;

class Notification extends AbstractComponent implements VolatileInterface
{
    // ... properties and methods
}</code>
</pre>

In future there will be native components implemented that marked as volatile to optimize processing speed & memory consumption since they don't need to be managed by the server.
