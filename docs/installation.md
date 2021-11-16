---
layout: post
title: Installation
published: true
---
<h1 class="doc-title">Installation and setup</h1>

- [Installation](#Installation)

<h4><a id="installation"></a></h4>
# Installation

Though most of the installation is done automatically, it requires some manual steps in order to fully initiate the Impulse PHP Framework. The first step is to create a symfony project by using the following command on command line:

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
	<code class="language-bash">composer create-project symfony/website-skeleton project_name && cd project_name</code>
  </pre>
</div>

After the symfony project has been created, you need to install the WebpackEncoreBundle.

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
	<code class="language-bash">composer require symfony/webpack-encore-bundle</code>
  </pre>
</div>

Since the ImpulseBundle is still a private repository and not available via packagist, the bundle must point to its repostory by adding the following segments to the composer json:

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
  <pre class="code-white imp-code line-numbers language-json">
	<code class="language-json">    "repositories": {
        "impulsebundle": {
            "type": "path",
            "url": "/Users/rudolph/Sites/localhost/impulseproject/impulsebundle"
        }
    },
    
    "require": {
        "impulsephp/impulsebundle": "dev-FbWebsockets@dev",
    }</code>
  </pre>
</div>


yooooooooooo

Once this procedure is finished, you need to install node.js (if you haven't already). If you haven't installed it, you can follow these instructions: https://nodejs.org/en/download/package-manager/. 

After that, we can execute the installation command to install the modules defined in the package.json file. 

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
	<code class="language-bash">npm install</code>
  </pre>
</div>

Furthermore you need to install the jquery and the sass-loader libraries by using the following command.

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
	<code class="language-bash">npm install --save jquery && npm install sass-loader@^9.0.1 --save-dev</code>
  </pre>
</div>

After that, you should erase everything from the assets directory since the Impulse flex receipe will place all required files within that directory.

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
	<code class="language-bash">rm -rf ./assets/*</code>
  </pre>
</div>

The next step is installing the Impulse PHP Framework bundle itself.

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
	<code class="language-bash">composer require impulsephp/impulsebundle:dev-master@dev</code>
  </pre>
</div>

Lastly you can remove the webpack.config.js and rename the webpack_impulse.config.js to webpack.config.js. 

After that, you should be able to successfully run 

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
	<code class="language-bash">npm run dev</code>
  </pre>
</div>

You can now open the web application via browser (e.g. http://localhost/project/public/). You should see the example entry page.
