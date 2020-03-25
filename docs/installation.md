---
layout: post
title: Installation
published: true
---
<h1 class="doc-title">Installation and setup</h1>

- [Download](#download)
- [Installation](#Installation)
- [Authentication example](#authentication-example)

<a name="download"></a>
# Download

As long as the package is not public, you have to download the installer manually. You can download the packahe <a href="downloads/installer.zip" target="_blank">here</a>.

<a name="installation"></a>
# Installation

For a simple installation you can unpack the downloaded zip, rename the directory (if you want) and afterwards you have to run the following command.

<div>
  <div class="code-header">
    <div class="container-fluid">
        <div class="row">
            <div class="button red" />
          	<div class="button yellow" />
          	<div class="button green" />
        </div>
    </div>
  </div>
	<pre class="code-white imp-code line-numbers language-shell">
		<code class="language-bash">composer install</code>
	</pre>
</div>

The procedure will ask you for the symfony apache pack if you want to use, type n. Once the procedure is finished, you can open the web application via browser (e.g. http://localhost/project/public/). You should see the example entry page.

<a name="authentication-example"></a>
# Authentication example