<h1 class="doc-title">Setup</h1>

- [Security.yaml](#security-yaml)
- [Installation](#Installation)
- [Authentication example](#authentication-example)


<a name="security-yaml"></a>
<h4>Security.yaml</h4>
  
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
  <pre class="code-white imp-code line-numbers language-yaml">
	<code class="language-yaml">security:
		encoders:
			App\Entity\User:
				algorithm: bcrypt
				cost: 4
            
		providers:
			database_users:
				entity: { class: App\Entity\User, property: username }
            
		firewalls:
		# ...
			main:
				# ...
				provider: database_users

				logout:
					path: logout</code>
  </pre>
</div>

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

The procedure will ask you for the symfony apache pack if you want to use, type n. After a successful installation you have to run the following command in order to make impulse work.

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
	<code class="language-bash">php bin/console impulse:setup</code>
  </pre>
</div>


Once the procedure is finished, you can open the web application via browser (e.g. http://localhost/project/public/). You should see the example entry page.

<a name="authentication-example"></a>
# Authentication example

You can generate a very basic authentication example. First of all, you have to adjust the doctrine.yaml file with the following configurations.

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

An empty sqlite database is already provided in the var/ directory. The next step is adjusting the database connection settings in the .env file.

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
	<code class="language-bash">DATABASE_URL=sqlite:///%kernel.project_dir%/var/data.db</code>
  </pre>
</div>

The last step is just generating the authentication example with the following command.

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
	<code class="language-bash">php bin/console make:impulse:auth</code>
  </pre>
</div>

After that, refresh the main page and you will see that "Login" and "Registration" menu entries have been added automatically.
