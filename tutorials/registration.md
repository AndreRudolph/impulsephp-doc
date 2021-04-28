<h1 class="doc-title">Registration</h1>

- [Registration template](#registration-template)
- [Registration controller](#registration-controller)


<a name="registration-template"></a>
<h4>Registration template</h4>

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

<a name="registration-controller"></a>
<h4>Registration controller</h4>

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
use App\Entity\User;
use App\Service\UserService;
use Impulse\ImpulseBundle\Controller\AbstractController;
use Impulse\ImpulseBundle\Controller\Annotations\Listen;
use Impulse\ImpulseBundle\Controller\Annotations\Transient;
use Impulse\ImpulseBundle\Events\Events;
use Impulse\ImpulseBundle\UI\Components\FeedbackTextbox;
use Impulse\ImpulseBundle\Execution\Events\Event;
use Impulse\ImpulseBundle\UI\Components\Modal;

class RegistrationController extends AbstractController
{
    private Modal $wndRegistration;
    private FeedbackTextbox $tbUsername;
    private FeedbackTextbox $tbPassword;
    private FeedbackTextbox $tbPasswordRepeat;
    private FeedbackTextbox $tbEmail;

    #[Transient] private UserService $userService;

    public function __construct(UserService $userService)
    {
        $this->userService = $userService;
    }

    public function afterCreate(Event $event)
    {
        parent::afterCreate($event);
        if ($this->isGranted('IS_AUTHENTICATED_FULLY')) {
            return $this->redirectTo('_impulse_index');
        }
    }

    #[Listen(event: Events::CLICK, component: 'btnRegister')]
    public function onRegister(): void
    {
        $this->resetErrors();

        if ($this->validate()) {
            $this->doRegistration();
        }
    }

    #[Listen(event: Events::CLICK, component: 'btnClose')]
    #[Listen(event: Events::CLOSE, component: 'wndRegistration')]
    public function onClose($event): void
    {
        $this->wndRegistration->close();
    }

    private function doRegistration(): void
    {
        $username = $this->tbUsername->getValue();
        $password = $this->tbPassword->getValue();
        $email = $this->tbEmail->getValue();

        $user = (new User())
            ->setUsername($username)
            ->setPassword($password)
            ->setEmail($email);

        $this->userService->save($user);

        $this->notifySuccess()
            ->header('User registered')
            ->message(sprintf('User %s has been registered.', $username))
            ->create();

        $this->wndRegistration->close();
    }

    private function validate(): bool
    {
        $valid = true;

        // username
        $valid &= $this->validateTextbox($this->tbUsername, static fn($value) => $value !== null && $value !== '', 'Username must not be empty!');
        $valid &= $this->validateTextbox($this->tbUsername, function($value) {
            return !$this->userService->usernameExists($value);
        }, 'User already taken!');

        // password
        $valid &= $this->validateTextbox($this->tbPassword, static fn($value) => $value !== null && $value !== '', 'Password must not be empty!');
        $valid &= $this->validateTextbox($this->tbPasswordRepeat, function($value) {
            return $value === $this->tbPassword->getValue();
        },'Passwords must be equal!');

        // email
        $valid &= $this->validateTextbox($this->tbEmail, fn($value) => filter_var($value, FILTER_VALIDATE_EMAIL), 'No valid email.');
        $valid &= $this->validateTextbox($this->tbEmail, function($value) {
            return !$this->userService->emailExists($value);
        }, 'Email is already taken.');

        return $valid;
    }

    private function validateTextbox(FeedbackTextbox $textbox, callable $validator, string $message): bool
    {
        if (!$validator($textbox->getValue() ?? '')) {
            $textbox->setFeedback(FeedbackTextbox::FEEDBACK_INVALID, $message);
            return false;
        }

        return true;
    }

    private function resetErrors(): void
    {
        $this->tbUsername->resetFeedback();
        $this->tbPassword->resetFeedback();
        $this->tbPasswordRepeat->resetFeedback();
        $this->tbEmail->resetFeedback();
    }
}</code>
  </pre>
</div>
