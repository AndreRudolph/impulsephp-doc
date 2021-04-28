<h1 class="doc-title">Setup</h1>

- [Security.yaml](#security-yaml)
- [User entity](#user-entity)
- [Authentication provider](#authentication-provider)


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

<a name="user-entity"></a>
<h4>User entity</h4>

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

<a name="authentication-provider"></a>
<h4>Authentication provider</h4>

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
namespace App\Security;

use Symfony\Component\Security\Core\Authentication\Provider\UserAuthenticationProvider;
use Symfony\Component\Security\Core\Authentication\Token\UsernamePasswordToken;
use Symfony\Component\Security\Core\Encoder\UserPasswordEncoderInterface;
use Symfony\Component\Security\Core\Exception\AuthenticationException;
use Symfony\Component\Security\Core\Exception\AuthenticationServiceException;
use Symfony\Component\Security\Core\Exception\UsernameNotFoundException;
use Symfony\Component\Security\Core\User\UserCheckerInterface;
use Symfony\Component\Security\Core\User\UserInterface;
use Symfony\Component\Security\Core\User\UserProviderInterface;

/**
 * author André Rudolph <rudolph[at]impulse-php.com>
 */
class DatabaseAuthenticationProvider extends UserAuthenticationProvider
{
    private UserPasswordEncoderInterface $passwordEncoder;
    private UserProviderInterface $userProvider;

    public function __construct(UserPasswordEncoderInterface $passwordEncoder, UserProviderInterface $userProvider, UserCheckerInterface $userChecker, string $providerKey, bool $hideUserNotFoundExceptions = true)
    {
        parent::__construct($userChecker, $providerKey, $hideUserNotFoundExceptions);
        $this->userProvider = $userProvider;
        $this->passwordEncoder = $passwordEncoder;
    }

    protected function retrieveUser(string $username, UsernamePasswordToken $token): UserInterface
    {
        $user = $token->getUser();

        if ($user instanceof UserInterface) {
            return $user;
        }

        try {
            $user = $this->userProvider->loadUserByUsername($username);

            if (!$user instanceof UserInterface) {
                throw new AuthenticationServiceException('The user provider must return a UserInterface object.');
            }

            return $user;
        } catch (UsernameNotFoundException $e) {
            $e->setUsername($username);
            throw $e;
        } catch (\Exception $e) {
            $e = new AuthenticationServiceException($e->getMessage(), 0, $e);
            $e->setToken($token);
            throw $e;
        }
    }

    protected function checkAuthentication(UserInterface $user, UsernamePasswordToken $token): void
    {
        $currentUser = $token->getUser();

        if ($currentUser instanceof UserInterface) {
            if ($currentUser->getPassword() !== $user->getPassword()) {
                throw new AuthenticationException('Credentials were changed from another session.');
            }
        } else {
            $password = $token->getCredentials();

            if (empty($password)) {
                throw new AuthenticationException('Password can not be empty.');
            }

            if (!$this->passwordEncoder->isPasswordValid($user, $password)) {
                throw new AuthenticationException('Password is invalid.');
            }
        }
    }
}</code>
  </pre>
</div>