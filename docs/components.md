<h3 class="doc-title">Components</h3>

- [Introduction](#introduction)
- [Basics](#basics)
	- [Show available components](#registered_components)
	- [Client-server-synchronization](#client_server_synchronization)
    - [Create custom components](#create_custom_components)
- [Learn more about components](#advanced_topics)

<h4><a id="introduction">Introduction</a></h4>

Components is the key concept behind the Impulse PHP Framework because literally everything is based on that. A component represents an object that itself is rendered as a HTML representation in the browser via the Impulse javascript engine. Components can be either trivial and atomar (e.g. a textbox) or even more complex like a caroussel component. Almost every native html tag is considered as a component itself. Unlike 'normal' (ajax based) web application such componnets have a state on client as well on server side.

Each component (and of course its state) is stored within the user's session. This is required to keep the state of a component alive as long as the component resides in the DOM. Every component should inherit directly or indirectly form the AbstractComponent since it implements the necessary basics to have a working component. Once a component has been created either on server side as well on client side, the component resides in an updatable state. This means that most of the components attributes are synchronized with the client like e.g. width, height (defined in AbstractComponent) or value for textbox components. Whenever you update on of the synchronizable attributes, the changes are directly recorded and part of the response of the server.

Like the html representatives, each component has exactly one parent (except root) and zero to any number of childs. When the session gets persisted or updated, the parent-/child relationships will also be stored.

<h4><a id="basics">Basics</a></h4>

<h5><a id="registered_components">Show available components</a></h5>

For a complete list of currently registered components you can run the following command in your projects root directory:

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
  <pre class="code-white imp-code line-numbers language-shell">
  	<code class="language-bash">php bin/console debug:impulse:components</code>
  </pre>
</div>

The output might look as the following:

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
  <pre class="code-white imp-code line-numbers language-bash">
  	<code class="language-bash"> ------------------------- ----------------------------------------------------- 
    Alias                     Class                                                
   ------------------------- ----------------------------------------------------- 
    a                         Impulse\ImpulseBundle\UI\Components\A                
    impulse:a                 Impulse\ImpulseBundle\UI\Components\A                
    blockquote                Impulse\ImpulseBundle\UI\Components\Blockquote       
    impulse:blockquote        Impulse\ImpulseBundle\UI\Components\Blockquote       
    br                        Impulse\ImpulseBundle\UI\Components\Br               
    impulse:br                Impulse\ImpulseBundle\UI\Components\Br               
    button                    Impulse\ImpulseBundle\UI\Components\Button           
    impulse:button            Impulse\ImpulseBundle\UI\Components\Button           
    carousel                  Impulse\ImpulseBundle\UI\Components\Carousel         
    impulse:carousel          Impulse\ImpulseBundle\UI\Components\Carousel         
    carouselitem              Impulse\ImpulseBundle\UI\Components\CarouselItem     
    impulse:carouselitem      Impulse\ImpulseBundle\UI\Components\CarouselItem     
    checkbox                  Impulse\ImpulseBundle\UI\Components\Checkbox         
    impulse:checkbox          Impulse\ImpulseBundle\UI\Components\Checkbox         
    combobox                  Impulse\ImpulseBundle\UI\Components\Combobox         
    impulse:combobox          Impulse\ImpulseBundle\UI\Components\Combobox         
    comboitem                 Impulse\ImpulseBundle\UI\Components\Comboitem        
    impulse:comboitem         Impulse\ImpulseBundle\UI\Components\Comboitem        
    datacombobox              Impulse\ImpulseBundle\UI\Components\DataCombobox     
    impulse:datacombobox      Impulse\ImpulseBundle\UI\Components\DataCombobox     
    div                       Impulse\ImpulseBundle\UI\Components\Div              
    impulse:div               Impulse\ImpulseBundle\UI\Components\Div              
    feedbacktextbox           Impulse\ImpulseBundle\UI\Components\FeedbackTextbox  
    impulse:feedbacktextbox   Impulse\ImpulseBundle\UI\Components\FeedbackTextbox  
    footer                    Impulse\ImpulseBundle\UI\Components\Footer           
    impulse:footer            Impulse\ImpulseBundle\UI\Components\Footer           
    form                      Impulse\ImpulseBundle\UI\Components\Form             
    impulse:form              Impulse\ImpulseBundle\UI\Components\Form             
    h1                        Impulse\ImpulseBundle\UI\Components\H1               
    impulse:h1                Impulse\ImpulseBundle\UI\Components\H1               
    h2                        Impulse\ImpulseBundle\UI\Components\H2               
    impulse:h2                Impulse\ImpulseBundle\UI\Components\H2               
    h3                        Impulse\ImpulseBundle\UI\Components\H3               
    impulse:h3                Impulse\ImpulseBundle\UI\Components\H3               
    h4                        Impulse\ImpulseBundle\UI\Components\H4               
    impulse:h4                Impulse\ImpulseBundle\UI\Components\H4               
    h5                        Impulse\ImpulseBundle\UI\Components\H5               
    impulse:h5                Impulse\ImpulseBundle\UI\Components\H5               
    h6                        Impulse\ImpulseBundle\UI\Components\H6               
    impulse:h6                Impulse\ImpulseBundle\UI\Components\H6               
    hbox                      Impulse\ImpulseBundle\UI\Components\Hbox             
    impulse:hbox              Impulse\ImpulseBundle\UI\Components\Hbox             
    header                    Impulse\ImpulseBundle\UI\Components\Header           
    impulse:header            Impulse\ImpulseBundle\UI\Components\Header           
    hr                        Impulse\ImpulseBundle\UI\Components\Hr               
    impulse:hr                Impulse\ImpulseBundle\UI\Components\Hr               
    i                         Impulse\ImpulseBundle\UI\Components\I                
    impulse:i                 Impulse\ImpulseBundle\UI\Components\I                
    iframe                    Impulse\ImpulseBundle\UI\Components\Iframe           
    impulse:iframe            Impulse\ImpulseBundle\UI\Components\Iframe           
    image                     Impulse\ImpulseBundle\UI\Components\Image            
    impulse:image             Impulse\ImpulseBundle\UI\Components\Image            
    intbox                    Impulse\ImpulseBundle\UI\Components\Intbox           
    impulse:intbox            Impulse\ImpulseBundle\UI\Components\Intbox           
    label                     Impulse\ImpulseBundle\UI\Components\Label            
    impulse:label             Impulse\ImpulseBundle\UI\Components\Label            
    li                        Impulse\ImpulseBundle\UI\Components\Li               
    impulse:li                Impulse\ImpulseBundle\UI\Components\Li               
    listbody                  Impulse\ImpulseBundle\UI\Components\Listbody         
    impulse:listbody          Impulse\ImpulseBundle\UI\Components\Listbody         
    listbox                   Impulse\ImpulseBundle\UI\Components\Listbox          
    impulse:listbox           Impulse\ImpulseBundle\UI\Components\Listbox          
    listcell                  Impulse\ImpulseBundle\UI\Components\Listcell         
    impulse:listcell          Impulse\ImpulseBundle\UI\Components\Listcell         
    listhead                  Impulse\ImpulseBundle\UI\Components\Listhead         
    impulse:listhead          Impulse\ImpulseBundle\UI\Components\Listhead         
    listheader                Impulse\ImpulseBundle\UI\Components\Listheader       
    impulse:listheader        Impulse\ImpulseBundle\UI\Components\Listheader       
    listitem                  Impulse\ImpulseBundle\UI\Components\Listitem         
    impulse:listitem          Impulse\ImpulseBundle\UI\Components\Listitem         
    navbar                    Impulse\ImpulseBundle\UI\Components\Navbar           
    impulse:navbar            Impulse\ImpulseBundle\UI\Components\Navbar           
    navgroup                  Impulse\ImpulseBundle\UI\Components\Navgroup         
    impulse:navgroup          Impulse\ImpulseBundle\UI\Components\Navgroup         
    navitem                   Impulse\ImpulseBundle\UI\Components\Navitem          
    impulse:navitem           Impulse\ImpulseBundle\UI\Components\Navitem          
    navmenu                   Impulse\ImpulseBundle\UI\Components\Navmenu          
    impulse:navmenu           Impulse\ImpulseBundle\UI\Components\Navmenu          
    notification              Impulse\ImpulseBundle\UI\Components\Notification     
    impulse:notification      Impulse\ImpulseBundle\UI\Components\Notification     
    ol                        Impulse\ImpulseBundle\UI\Components\Ol               
    impulse:ol                Impulse\ImpulseBundle\UI\Components\Ol               
    p                         Impulse\ImpulseBundle\UI\Components\P                
    impulse:p                 Impulse\ImpulseBundle\UI\Components\P                
    paging                    Impulse\ImpulseBundle\UI\Components\Paging           
    impulse:paging            Impulse\ImpulseBundle\UI\Components\Paging           
    progressbar               Impulse\ImpulseBundle\UI\Components\Progressbar      
    impulse:progressbar       Impulse\ImpulseBundle\UI\Components\Progressbar      
    radio                     Impulse\ImpulseBundle\UI\Components\Radio            
    impulse:radio             Impulse\ImpulseBundle\UI\Components\Radio            
    radiogroup                Impulse\ImpulseBundle\UI\Components\RadioGroup       
    impulse:radiogroup        Impulse\ImpulseBundle\UI\Components\RadioGroup       
    section                   Impulse\ImpulseBundle\UI\Components\Section          
    impulse:section           Impulse\ImpulseBundle\UI\Components\Section          
    space                     Impulse\ImpulseBundle\UI\Components\Space            
    impulse:space             Impulse\ImpulseBundle\UI\Components\Space            
    span                      Impulse\ImpulseBundle\UI\Components\Span             
    impulse:span              Impulse\ImpulseBundle\UI\Components\Span             
    strong                    Impulse\ImpulseBundle\UI\Components\Strong           
    impulse:strong            Impulse\ImpulseBundle\UI\Components\Strong           
    tbody                     Impulse\ImpulseBundle\UI\Components\TBody            
    impulse:tbody             Impulse\ImpulseBundle\UI\Components\TBody            
    tfoot                     Impulse\ImpulseBundle\UI\Components\TFoot            
    impulse:tfoot             Impulse\ImpulseBundle\UI\Components\TFoot            
    thead                     Impulse\ImpulseBundle\UI\Components\THead            
    impulse:thead             Impulse\ImpulseBundle\UI\Components\THead            
    tab                       Impulse\ImpulseBundle\UI\Components\Tab              
    impulse:tab               Impulse\ImpulseBundle\UI\Components\Tab              
    tabbox                    Impulse\ImpulseBundle\UI\Components\Tabbox           
    impulse:tabbox            Impulse\ImpulseBundle\UI\Components\Tabbox           
    table                     Impulse\ImpulseBundle\UI\Components\Table            
    impulse:table             Impulse\ImpulseBundle\UI\Components\Table            
    tabpanel                  Impulse\ImpulseBundle\UI\Components\Tabpanel         
    impulse:tabpanel          Impulse\ImpulseBundle\UI\Components\Tabpanel         
    tabpanels                 Impulse\ImpulseBundle\UI\Components\Tabpanels        
    impulse:tabpanels         Impulse\ImpulseBundle\UI\Components\Tabpanels        
    tabs                      Impulse\ImpulseBundle\UI\Components\Tabs             
    impulse:tabs              Impulse\ImpulseBundle\UI\Components\Tabs             
    td                        Impulse\ImpulseBundle\UI\Components\Td               
    impulse:td                Impulse\ImpulseBundle\UI\Components\Td               
    textarea                  Impulse\ImpulseBundle\UI\Components\Textarea         
    impulse:textarea          Impulse\ImpulseBundle\UI\Components\Textarea         
    textbox                   Impulse\ImpulseBundle\UI\Components\Textbox          
    impulse:textbox           Impulse\ImpulseBundle\UI\Components\Textbox          
    th                        Impulse\ImpulseBundle\UI\Components\Th               
    impulse:th                Impulse\ImpulseBundle\UI\Components\Th               
    tr                        Impulse\ImpulseBundle\UI\Components\Tr               
    impulse:tr                Impulse\ImpulseBundle\UI\Components\Tr               
    ul                        Impulse\ImpulseBundle\UI\Components\Ul               
    impulse:ul                Impulse\ImpulseBundle\UI\Components\Ul               
    upload                    Impulse\ImpulseBundle\UI\Components\Upload           
    impulse:upload            Impulse\ImpulseBundle\UI\Components\Upload           
    window                    Impulse\ImpulseBundle\UI\Components\Window           
    impulse:window            Impulse\ImpulseBundle\UI\Components\Window                             
   ------------------------- ----------------------------------------------------- 
</code>
  </pre>
</div>


Available components are discovered automatically and you don't have to register any component class by yourself as long as you follow the conventions. The convention is that all classes within the UI/Components/ directory which implement the ComponentContract interface are registered as components automatically.

<h5><a id="client_server_synchronization">Client-server-synchronization</a></h5>

The main purpose of the components is to store both the component at the client and at the server side. The logical conclusion is that server-side changes to a component that would affect its appeareance or behavior must be synchronized with the client (browser).

To achieve this, most of the setters for attributes (e.g. setHeight, setVisible, etc.) are observed for changes. This means whenever you set the height of the component after it was created and populated in a request before, the internal server side state of the components gets updated and the client receives an update aswell.

<h5><a id="create_own_components">Create custom components</a></h5>

Impulse is designed to provide programmers the possibility to create their own components for their very specific needs or to even share with other users of the Impulse framework. As previousely mentioned, a component can be either very basic and atomic components like a textbox or a label or can be even more sophisticated like even a wysiwyg editor.  

To support you, you can create a skeleton component with the following command.

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
	<pre class="code-white imp-code line-numbers language-shell">
		<code class="language-bash">php bin/console make:impulse:component</code>
	</pre>
</div>


<h5><a id="component_contexts">Component contexts</a></h5>

The ComponentClassRegistry is the lookup class in which all discovered components are registered. Remember the following command to list all available components.

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
  <pre class="code-white imp-code line-numbers language-shell">
  	<code class="language-bash">php bin/console debug:impulse:components</code>
  </pre>
</div>

As you maybe noticed each component is registered at least twice. For example the button component is registered as 'button' and 'impulse:button'. The reason behind this is that the Impulse framework offers you the opportunity to have multiple button components registered.

Consider the following scenario:
App/
	UI/
    	Components/
        	Button.php

In the example above in the App is another button component. The component scan discovery overwrites non contextual aliases with the last find. According to this, the following button

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
    <pre class="code-white line-numbers language-markup">
    <code class="imp-code language-markup">&lt;impulse&gt;
        &lt;button /&gt;
    &lt;/impulse&gt;</code>
    </pre>
</div>

will be bound to the app button component class. However, you can also explicitly use the button implementation that is provided by the Impulse framework with a contextual prefix.

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
  <pre class="code-white line-numbers language-markup">
    <code class="imp-code language-markup">&lt;impulse&gt;
        &lt;impulse:button /&gt;
    &lt;/impulse&gt;</code>
  </pre>
</div>

<a name="advanced_topics">Learn more about components</a>

<ul class="unstyled-list">
  <li><a data-target-menu-item="component_lifecycle" class="text-muted">Component lifecycle</a></li>
  <li><a data-target-menu-item="component_service_wiring">Components Service Wiring</a></li>
  <li><a data-target-menu-item="components_el_wiring">Components Expression Language Based Wiring</a></li>
</ul>
