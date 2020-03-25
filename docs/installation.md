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

You can generate a very basic authentication example. First of all, you have to adjust the doctrine.yaml file with the following configurations.

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
	<pre class="code-white imp-code line-numbers language-yaml">
		<code class="language-yaml">parameters:
			env(DATABASE_URL): ''
        doctrine:
			dbal:
				# configure these for your database server
				driver: 'pdo_mysql'
				server_version: '5.7'
				charset: utf8mb4
				default_table_options:
					charset: utf8mb4
					collate: utf8mb4_unicode_ci
				url: '%env(resolve:DATABASE_URL)%'
		orm:
			auto_generate_proxy_classes: true
			naming_strategy: doctrine.orm.naming_strategy.underscore
			auto_mapping: true
			mappings:
			App:
				is_bundle: false
				type: annotation
				dir: '%kernel.project_dir%/src/Entity'
				prefix: 'App\Entity'
				alias: App
</code>
	</pre>
</div>
