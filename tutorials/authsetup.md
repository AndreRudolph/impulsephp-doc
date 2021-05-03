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
	- [Login template](#login-template)
    - [Authentication provider](#authentication-provider)
	- [Authentication service](#authentication-service)
	- [Login controller](#login-controller)
    
 
<h4><a id="introduction">Introduction</a></h4>

Both the tutorials for registration and authentication are based on the following setup steps which define a simple but yet working base for an authentication system with registration, login and logout functionality. 

Before we dig into these tutorials the following setup steps are required.

<h4><a id="setup">Setup</h4>

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

<a name="user-entity"></a>
<h4>User entity</h4>

The user entity is the main model that represents a user within your application. The class must implement the UserInterface and must implement the required methods. In this example we do not add complexity of user roles since this tutorial will be kept simple and we just return the admin role hard coded.

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

<a name="user-service"></a>
<h4>User service</h4>

The user service is the central access point to the user entities of the database.

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
use App\Entity\User;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Component\Security\Core\Encoder\UserPasswordEncoder;
use Symfony\Component\Security\Core\Encoder\UserPasswordEncoderInterface;
use Symfony\Component\Security\Core\Exception\UnsupportedUserException;
use Symfony\Component\Security\Core\User\UserInterface;
use Symfony\Component\Security\Core\User\UserProviderInterface;

class UserService implements UserProviderInterface
{
    public function loadUserByUsername(string $username)
    {
        
    }

    public function supportsClass(string $class)
    {
        
    }

    public function refreshUser(UserInterface $user)
    {
        
    }
}</code>
  </pre>
</div>

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
use App\Entity\User;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Component\Security\Core\Encoder\UserPasswordEncoder;
use Symfony\Component\Security\Core\Encoder\UserPasswordEncoderInterface;
use Symfony\Component\Security\Core\Exception\UnsupportedUserException;
use Symfony\Component\Security\Core\User\UserInterface;
use Symfony\Component\Security\Core\User\UserProviderInterface;

class UserService implements UserProviderInterface
{
    private $em;
    private $passwordEncoder;

    public function __construct(EntityManagerInterface $em, UserPasswordEncoderInterface $passwordEncoder)
    {
        $this->em = $em;
        $this->passwordEncoder = $passwordEncoder;
    }

    public function loadUserByUsername(string $username)
    {
        $result = $this->findByAttribute('username', $username);
        if (!is_array($result) || empty($result)) {
            return null;
        }

        return $result[0];
    }

    public function supportsClass(string $class)
    {
        return $class === User::class;
    }

    public function refreshUser(UserInterface $user)
    {
        if (!$user instanceof User) {
            throw new UnsupportedUserException(sprintf('Instances of "%s" are not supported.', get_class($user)));
        }

        return $this->loadUserByUsername($user->getUsername());
    }

    public function findByAttribute(string $attribute, $value): ?array
    {
        return $this->em->getRepository(User::class)
            ->findBy([$attribute => $value]);
    }
}</code>
  </pre>
</div>
