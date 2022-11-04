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

<pre class="language-shell code-white imp-code">
    <code class="language-shell">
composer create-project symfony/website-skeleton project_name && cd project_name</code>
</pre>

After the symfony project has been created, you need to install the WebpackEncoreBundle.

<pre class="language-shell code-white imp-code language-shell">
    <code class="language-shell">
composer require symfony/webpack-encore-bundle</code>
</pre>

Since the ImpulseBundle is still a private repository and not available via packagist, the bundle must point to its repostory by adding the following segments to the composer json:

<pre class="language-json code-white imp-code language-shell">
    <code class="language-json">    
"repositories": {
    "impulsebundle": {
        "type": "vcs",
        "url": "https://Atomtastic@bitbucket.org/Atomtastic/impulsebundle.git"
    }
},

"require": {
    "impulsephp/impulsebundle": "dev-master@dev",
}</code>
</pre>

Now it's time to let composer install that package by running the update command.

<pre class="code-white imp-code language-shell">
    <code class="language-bash">
composer update</code>
</pre>

The ImpulseBundle is now available. To finally initialize it, you can execute the provided init command and 
following the instructions once the command has been finished.

<pre class="language-shell code-white imp-code language-shell">
    <code class="language-shell">
php bin/console impulse:init</code>
</pre>

You can now open the web application via browser (e.g. http://localhost/project/public/). You should see the example entry page.
