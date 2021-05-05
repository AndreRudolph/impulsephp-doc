---
layout: post
title: Installation
published: true
---
<h1 class="doc-title">Installation and setup</h1>

- [Download](#download)
- [Installation](#Installation)

<a name="download"></a>
# Download

As long as the package is not public, you have to download the installer manually. You can download the package <a href="downloads/installer.zip" target="_blank">here</a>.

<a name="installation"></a>
# Installation

For a simple installation you can unpack the downloaded zip, rename the directory (if you want) and afterwards you have to run the following command.

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
	<code class="language-bash">composer install</code>
  </pre>
</div>

The procedure will ask you for the symfony apache pack if you want to use, type n. 

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


Once the procedure is finished, you can open the web application via browser (e.g. http://localhost/project/public/). You should see the example entry page.
