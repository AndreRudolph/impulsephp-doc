<h1 class="doc-title">Configuration reference</h1>

- [Introduction](#introduction)
- [Configuration reference](#config_reference)

<a name="introduction"></a>
<h4>Introduction</h4>
In this chapter all available configuration parameters of the Impulse bundle will be explained in depth and a general overview is shown at the end of this documentation page.

<a name="config_reference"></a>
<h4>Configuration reference</h4>

The following configuration is a complete example.

<pre class="code-white imp-code line-numbers language-yaml">
<code class="language-yaml">impulse:
client:
    bundles: [ 'ImpulseBundle' ]

#page:
#
#   integrity_check_enabled: true (default: false)
#
#

encryption:

    # Specifies the cipher that will be used for openssl_encrypt and openssl_decrypt.
    cipher: 'AES-256-CBC'

    # The secret key that will be used for encryption. By default the symfon app secret is used but can be changed
    # to a custom key.
    secret: '%env(APP_SECRET)%'

    # The hash algorithm that is used to compute the signature of the UI model.
    signature_hash_algorithm: 'sha256'

uids:

    # Defines a uid generation strategy for each symfony environment you use. Possible values are random or traceable.
    # If no strategy is defined for the current environment, the random strategy will be used. When using the random
    # strategy, both the scope_uid_length and component_uid_length are considered for random
    # uid generation. The traceable mode, instead, will created deterministic uids based on the following pattern:
    # {View_name}-{incremental_view_number}_{Component_class_name}-{internal-component-id|incremental_class_based_number} .
    generation_strategies:
        - { environment: 'prod', strategy: 'random' }
        #            - { environment: 'dev', strategy: 'traceable' }
        - { environment: 'dev', strategy: 'random' }

    # This value defines the character length that the scope uid will be. The uid will, by default, be generated
    # randomly upon an alphabet of letters and digits. A scope represents a specific view instance and should thus
    # be high enough to either avoid likely collisions and to cover the amount of different view instances.
    #
    # Be aware that a higher number will result in more reserved bytes for both session and request / response data.
    scope_uid_length: 3

    # This value defines the character length that the component uid will be. The uid will, by default, be generated
    # randomly upon an alphabet of letters and digits. The value should be high enough to avoid likely collisions
    # and to cover the amount of different components per scope.
    #
    # Be aware that a higher number will result in more reserved bytes for both session and request / response data.
    component_uid_length: 5</code>
</pre>
