<h1 class="doc-title">Authentication & registration</h1>

- [Introduction](#introduction)
- [Setup](#setup)
	- [Security.yaml](#security-yaml)
    - [User entity](#user-entity)
    - [User repository](#user-repository)
    - [User service](#user-service)
- [Registration](#registration)
	- [Registration template](#registration-template)
    - [User repository](#user-repository-registration)
    - [User service](#user-service-registration)
    - [Registration controller](#registration-controller)
- [Authentication](#authentication)
    - [Authentication provider](#authentication-provider)
	- [Authentication service](#authentication-service)
    - [Services.yaml](#services-yaml)
    - [Login template](#login-template)
	- [Login controller](#login-controller)
    
 
<h4><a id="introduction">Introduction</a></h4>

Both the tutorials for registration and authentication are based on the following setup steps which define a simple but yet working base for an authentication system with registration, login and logout functionality. 

Before we dig into these tutorials the following setup steps are required.

<h4><a id="setup">Setup</a></h4>

<a name="security-yaml"></a>
<h5>Security.yaml</h5>

The security.yaml file is provided automatically by Symfony by installing the SecurityBundle. However, in your security.yaml you need to specify the encoder configuration for the later added user entity. We use bcrypt with a cost of 4.

Additionally we need to specify a provider for the users and call it database_users. It references the full qualified namespace of the user entity aswell as the property which uniquely identifies an user.

Lastly the main firewall will be adjusted by using the previously defined provider and defining the logout path.
  
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

<h5><a id="user-entity">User entity</a></h5>

The user entity is the main model that represents a user within your application. The class must implement the UserInterface. In this example we do not add complexity of user roles since this tutorial will be kept simple and we just return the admin role hard coded.

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
  <pre class="code-white imp-code line-numbers language-php">
	<code class="language-php"><?php
namespace App\Entity;
use Symfony\Component\Security\Core\User\UserInterface;
use Doctrine\ORM\Mapping as ORM;

/**
* @ORM\Entity()
* @ORM\Table(name="user")
*/
class User implements UserInterface
{
    /**
    * @ORM\Id()
    * @ORM\Column(type="integer")
    * @ORM\GeneratedValue(strategy="AUTO")
    */
    private $id;

    /**
    * @ORM\Column(type="string", nullable=false)
    */
    private $username;

    /**
    * @ORM\Column(type="string")
    */
    private $password;

    /**
    * @ORM\Column(type="string")
    */
    private $email;

    /**
    * @return mixed
    */
    public function getId()
    {
        return $this->id;
    }

    public function getUsername()
    {
        return $this->username;
    }

    /**
    * @param string $username
    */
    public function setUsername(string $username): User
    {
        $this->username = $username;
        return $this;
    }

    /**
    * @return mixed
    */
    public function getEmail()
    {
        return $this->email;
    }

    /**
    * @param mixed $email
    */
    public function setEmail(string $email): User
    {
        $this->email = $email;
        return $this;
    }

    public function getSalt()
    {
        return '';
    }

    public function eraseCredentials()
    {
        // TODO: Implement eraseCredentials() method.
    }

    public function getPassword()
    {
        return $this->password;
    }

    /**
    * @param string $password
    */
    public function setPassword(string $password): User
    {
        $this->password = $password;
        return $this;
    }

    public function getRoles()
    {
        return ['admin'];
    }
}</code>
  </pre>
</div>

<h5><a id="user-repository">User repository</a></h5>

The repository is the class directly coupled to the entity manager and thus should contain the data access logic. 

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
  <pre class="code-white imp-code line-numbers language-php">
	<code class="language-php"><?php
namespace App\Repository;

use Doctrine\ORM\EntityManagerInterface;

/**
 * author André Rudolph <rudolph[at]impulse-php.com>
 */
class UserRepository
{
    private EntityManagerInterface $em;

    public function __construct(EntityManagerInterface $em)
    {
        $this->em = $em;
    }
}</code>
  </pre>
</div>

This class will be extended by several methods in the upcoming chapters.

<h5><a id="user-service">User service</a></h5>

The user service should contain all business relevant logic to users. As of now the service only has the UserRepository class as dependency.

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
  <pre class="code-white imp-code line-numbers language-php">
	<code class="language-php"><?php
namespace App\Service;

use App\Repository\UserRepository;

class UserService implements UserProviderInterface
{
    private UserRepository $userRepository;

    public function __construct(UserRepository $userRepository)
    {
        $this->userRepository = $userRepository;
    }
}</code>
  </pre>
</div>

This class will be extended by several methods in the upcoming chapters.

<h4><a id="registration">Registration</a></h4>

<h5><a id="registration-template">Registration template</a></h5>

The registration form is placed inside a modal window but this is just optional. It contains input fields for username, password and its repetition and for the users email.

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
  <pre class="code-white imp-code line-numbers language-markup">
	<code class="language-markup">&lt;impulse&gt;
	&lt;modal id="wndRegistration" bind="App\Controller\Auth\RegistrationController"&gt;
		&lt;modalheader&gt;Login&lt;/modalheader&gt;
		&lt;modalbody&gt;
			&lt;div class="container mt-3"&gt;
				&lt;div class="row justify-content-center align-items-center"&gt;
					&lt;div class="col-12"&gt;
						&lt;div class="form-group"&gt;
							&lt;span&gt;Username&lt;/span&gt;
							&lt;feedbacktextbox id="tbUsername" /&gt;
						&lt;/div>
                        &lt;div class="form-group"&gt;
							&lt;span&gt;Password&lt;/span&gt;
							&lt;feedbacktextbox id="tbPassword" inputType="password" /&gt;
						&lt;/div>
						&lt;div class="form-group"&gt;
							&lt;span&gt;Password repeat&lt;/span&gt;
							&lt;feedbacktextbox id="tbPasswordRepeat" inputType="password" /&gt;
						&lt;/div&gt;
						&lt;div class="form-group"&gt;
							&lt;span&gt;E-Mail&lt;/span&gt;
							&lt;feedbacktextbox id="tbEmail" /&gt;
						&lt;/div&gt;
					&lt;/div&gt;
				&lt;/div&gt;
			&lt;/div&gt;
		&lt;/modalbody&gt;
		&lt;modalfooter&gt;
			&lt;button id="btnClose" class="btn btn-secondary"&gt;Close&lt;/button&gt;
			&lt;button id="btnRegister" class="btn btn-primary"&gt;Register&lt;/button&gt;
		&lt;/modalfooter&gt;
	&lt;/modal&gt;
&lt;/impulse&gt;</code>
  </pre>
</div>