<h1 class="doc-title">Authentication</h1>

- [Authentication provider](#authentication-provider)
- [Authentication service](#authentication-service)
- [Login template](#login-template)
- [Login controller](#login-controller)


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

<a name="authentication-service"></a>
<h4>Authentication service</h4>

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

use Symfony\Component\Security\Core\Authentication\AuthenticationProviderManager;
use Symfony\Component\Security\Core\Authentication\Token\Storage\UsageTrackingTokenStorage;
use Symfony\Component\Security\Core\Authentication\Token\TokenInterface;
use Symfony\Component\Security\Core\Authentication\Token\UsernamePasswordToken;
use Symfony\Component\Security\Core\Exception\AuthenticationServiceException;

/**
 * author André Rudolph <rudolph[at]impulse-php.com>
 */
class AuthenticationService
{
    /**
     * The firewall to which the service will authenticate.
     *
     * @var string
     */
    protected string $firewallName = 'main';

    private AuthenticationProviderManager $authenticationProviderManager;
    private UsageTrackingTokenStorage $tokenStorage;

    public function __construct(UsageTrackingTokenStorage $tokenStorage, AuthenticationProviderManager $authenticationProviderManager)
    {
        $this->authenticationProviderManager = $authenticationProviderManager;
        $this->tokenStorage = $tokenStorage;
    }

    /**
     * @param string $identity
     * @param string $password
     * @return TokenInterface|null
     */
    public function authenticate(string $identity, string $password): ?TokenInterface
    {
        try {
            $unauthenticatedToken = new UsernamePasswordToken($identity, $password, $this->firewallName);
            $authenticatedToken = $this->authenticationProviderManager->authenticate($unauthenticatedToken);
            $this->tokenStorage->setToken($authenticatedToken);
            return $authenticatedToken;
        } catch (AuthenticationServiceException $e) {
            return null;
        }
    }
}</code>
  </pre>
</div>

<a name="login-template"></a>
<h4>Login template</h4>

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
    &lt;modal id="wndAuth" bind="App\Controller\Auth\LoginController"&gt;
        &lt;modalheader&gt;
            &lt;span&gt;Login</span&gt;
        &lt;/modalheader&gt;
        &lt;modalbody&gt;
            &lt;span&gt;Username&lt;/span&gt;
            &lt;feedbacktextbox id="tbUsername" /&gt;
            &lt;span&gt;Password&lt;/span&gt;
            &lt;feedbacktextbox id="tbPassword" inputType="password" /&gt;
        &lt;/modalbody&gt;
        &lt;modalfooter&gt;
            &lt;button id="btnClose" class="btn btn-secondary"&gt;Close&lt;/button&gt;
            &lt;button id="btnLogin" class="btn btn-primary"&gt;Login&lt;/button&gt;
        &lt;/modalfooter&gt;
    &lt;/modal&gt;
&lt;/impulse&gt;</code>
  </pre>
</div>

<a name="login-controller"></a>
<h4>Login controller</h4>

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

namespace App\Controller\Auth;

use App\Service\AuthenticationService;
use Exception;
use Impulse\ImpulseBundle\Controller\AbstractController;
use Impulse\ImpulseBundle\Controller\Annotations\Listen;
use Impulse\ImpulseBundle\Controller\Annotations\Transient;
use Impulse\ImpulseBundle\Events\Events;
use Impulse\ImpulseBundle\Execution\Events\ClickEvent;
use Impulse\ImpulseBundle\UI\Components\FeedbackTextbox;
use Impulse\ImpulseBundle\UI\Components\Modal;

/**
 * author André Rudolph <rudolph[at]impulse-php.com>
 */
class LoginController extends AbstractController
{
    private Modal $wndAuth;
    private FeedbackTextbox $tbUsername;
    private FeedbackTextbox $tbPassword;

    #[Transient]
    private AuthenticationService $authenticationService;

    public function __construct(AuthenticationService $authenticationService)
    {
        $this->authenticationService = $authenticationService;
    }

    #[Listen(event: Events::CLICK, component: 'btnLogin')]
    public function onLogin(ClickEvent $event): void
    {
        $username = $this->tbUsername->getValue() ?? '';
        $password = $this->tbPassword->getValue() ?? '';

        try {
            $authenticatedToken = $this->authenticationService->authenticate($username, $password);

            if ($authenticatedToken !== null) {
                $this->notifySuccess()->duration(5000)->message('You have successfully been logged in.')->create();
                $this->close();
            } else {
                $this->handleInvalidCredentials();
            }
        } catch (Exception $e) {
            $this->handleInvalidCredentials();
        }
    }

    private function handleInvalidCredentials(): void
    {
        $this->tbUsername->setFeedback(FeedbackTextbox::FEEDBACK_INVALID, 'Invalid credentials.');
        $this->tbPassword->setFeedback(FeedbackTextbox::FEEDBACK_INVALID, 'Invalid credentials.');
    }

    #[Listen(event: Events::CLICK, component: 'btnClose')]
    #[Listen(event: Events::CLOSE, component: 'wndAuth')]
    public function onClose($event): void
    {
        $this->close();
    }

    private function close(): void
    {
        $this->wndAuth->close();
    }
}</code>
  </pre>
</div>
