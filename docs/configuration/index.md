## Security Settings

### Configuration Overview

The settings for the authentication service are defined in the `WAANVERSE_AUTH_CONFIG` dictionary. This dictionary allows developers to customize the behavior of various components, including security, cookie handling, multi-factor authentication, user configurations, and more.

Some configuration settings are optimized for specific environments, such as production or development. It is the developer’s responsibility to determine which settings are most appropriate for their use case and environment. For example, in a production environment, security-related settings like cookie security and token expiration times may require stricter configurations, while development environments may prioritize convenience over security for testing purposes.

Ensure that the correct configurations are applied based on the environment to maintain a secure and efficient authentication flow.

-   **`PUBLIC_KEY_PATH`**

    -   Type: `str`
    -   Required: `True`
    -   Default: None
    -   Description: The file path to the pem public key file used for verifying tokens.

-   **`CLOUDFLARE_TURNSTILE_SECRET_KEY`**

    -   Type: `str`
    -   Required: `False`
    -   Default: None
    -   Description: The secret key for Cloudflare Turnstile for captcha.

-   **`PRIVATE_KEY_PATH`**

    -   Type: `str`
    -   Required: `True`
    -   Default: None
    -   Description: The file path to the private key used for signing tokens.

-   **`USER_ID_CLAIM`**
    -   Type: `str`
    -   Required: `False`
    -   Default: `"id"`
    -   Description: The claim in the token that identifies the user.

---

## Cookie Configuration

-   **`ACCESS_TOKEN_COOKIE_NAME`**

    -   Type: `str`
    -   Required: `False`
    -   Default: `"access_token"`
    -   Description: The name of the cookie storing access tokens.

-   **`REFRESH_TOKEN_COOKIE_NAME`**

    -   Type: `str`
    -   Required: `False`
    -   Default: `"refresh_token"`
    -   Description: The name of the cookie storing refresh tokens.

-   **`COOKIE_PATH`**

    -   Type: `str`
    -   Required: `False`
    -   Default: `"/"`
    -   Description: The path for which cookies are valid.

-   **`COOKIE_DOMAIN`**

    -   Type: `Optional[str]`
    -   Required: `False`
    -   Default: `None`
    -   Description: The domain for which cookies are valid.

-   **`COOKIE_SAMESITE_POLICY`**

    -   Type: `str`
    -   Required: `False`
    -   Default: `"Lax"`
    -   Description: The SameSite policy for cookies. Valid options: `"Strict"`, `"Lax"`, `"None"`.

-   **`COOKIE_SECURE`**

    -   Type: `bool`
    -   Required: `False`
    -   Default: `False`
    -   Description: Whether cookies should only be transmitted over HTTPS.

-   **`COOKIE_HTTP_ONLY`**

    -   Type: `bool`
    -   Required: `False`
    -   Default: `True`
    -   Description: Whether cookies should be inaccessible to JavaScript.

-   **`ACCESS_TOKEN_COOKIE_MAX_AGE`**

    -   Type: `timedelta`
    -   Required: `False`
    -   Default: `30 minutes`
    -   Description: The maximum age of the access token cookie.

-   **`REFRESH_TOKEN_COOKIE_MAX_AGE`**
    -   Type: `timedelta`
    -   Required: `False`
    -   Default: `30 days`
    -   Description: The maximum age of the refresh token cookie.

---

## Multi-Factor Authentication (MFA)

-   **`MFA_RECOVERY_CODE_COUNT`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `10`
    -   Description: The number of recovery codes generated for MFA.

-   **`MFA_ISSUER_NAME`**

    -   Type: `str`
    -   Required: `False`
    -   Default: `"Authentication Service"`
    -   Description: The issuer name displayed in authentication apps.

-   **`MFA_CODE_LENGTH`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `6`
    -   Description: The length of the MFA code.

-   **`EMAIL_SECURITY_NOTIFICATIONS_ENABLED`**
    -   Type: `bool`
    -   Required: `False`
    -   Default: `True`
    -   Description: Whether email notifications are sent for Security events.

---

## User Configuration

-   **`USERNAME_MIN_LENGTH`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `4`
    -   Description: The minimum length for usernames.

-   **`USERNAME_MAX_LENGTH`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `20`
    -   Description: The maximum length for usernames.

-   **`RESERVED_USERNAMES`**
    -   Type: `List[str]`
    -   Required: `False`
    -   Default: `["admin", "administrator", "root", "system"]`
    -   Description: A list of reserved usernames that cannot be registered.

---

## Serializer Classes

-   **`BASIC_ACCOUNT_SERIALIZER`**

    -   Type: `str`
    -   Required: `False`
    -   Default: `"dj_waanverse_auth.serializers.base_serializers.BasicAccountSerializer"`
    -   Description: The serializer class for basic account information.

-   **`REGISTRATION_SERIALIZER`**
    -   Type: `str`
    -   Required: `False`
    -   Default: `"dj_waanverse_auth.serializers.signup_serializers.SignupSerializer"`
    -   Description: The serializer class for user registration.

---

## Email Settings

-   **`EMAIL_VERIFICATION_CODE_LENGTH`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `6`
    -   Description: The length of email verification codes.

-   **`EMAIL_VERIFICATION_CODE_IS_ALPHANUMERIC`**

    -   Type: `bool`
    -   Required: `False`
    -   Default: `False`
    -   Description: Whether email verification codes are alphanumeric.

-   **`EMAIL_SECURITY_NOTIFICATIONS_ENABLED`**

    -   Type: `bool`
    -   Required: `False`
    -   Default: `True`
    -   Description: Whether security notifications are sent via email.

-   **`EMAIL_THREADING_ENABLED`**

    -   Type: `bool`
    -   Required: `False`
    -   Default: `True`
    -   Description: Whether email operations use threading to improve performance.

-   **`BLACKLISTED_EMAILS`**

    -   Type: `List[str]`
    -   Required: `False`
    -   Default: `[]`
    -   Description: A list of blacklisted email addresses.

-   **`DISPOSABLE_EMAIL_DOMAINS`**

    -   Type: `List[str]`
    -   Required: `False`
    -   Default: `[]`
    -   Description: A list of disposable email domains that are not allowed.

-   **`EMAIL_BATCH_SIZE`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `50`
    -   Description: The batch size for email operations.

-   **`EMAIL_RETRY_ATTEMPTS`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `3`
    -   Description: The number of retry attempts for email delivery.

-   **`EMAIL_RETRY_DELAY`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `5`
    -   Description: The delay (in seconds) between email delivery retries.

-   **`EMAIL_MAX_RECIPIENTS`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `50`
    -   Description: The maximum number of recipients per email.

-   **`EMAIL_THREAD_POOL_SIZE`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `5`
    -   Description: The thread pool size for email operations.

-   **`VERIFICATION_EMAIL_SUBJECT`**

    -   Type: `str`
    -   Required: `False`
    -   Default: `"Verify your email address"`
    -   Description: The subject line for email verification messages.

-   **`VERIFICATION_EMAIL_CODE_EXPIRATION_TIME_MINUTES`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `15`
    -   Description: The expiration time for email verification codes (in minutes).

-   **`LOGIN_ALERT_EMAIL_SUBJECT`**

    -   Type: `str`
    -   Required: `False`
    -   Default: `"New login alert"`
    -   Description: The subject line for login alert emails.

---

## Password Reset

-   **`PASSWORD_RESET_CODE_EXPIRY_IN_MINUTES`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `10`
    -   Description: The expiration time for password reset codes (in minutes).

-   **`PASSWORD_RESET_CODE_LENGTH`**

    -   Type: `int`
    -   Required: `False`
    -   Default: `7`
    -   Description: The length of password reset codes.

-   **`PASSWORD_RESET_EMAIL_SUBJECT`**
    -   Type: `str`
    -   Required: `False`
    -   Default: `"Password reset request"`
    -   Description: The subject line for password reset emails.

---

## Admin Interface

-   **`ENABLE_ADMIN_PANEL`**

    -   Type: `bool`
    -   Required: `False`
    -   Default: `False`
    -   Description: Whether the admin panel is enabled.

-   **`USE_UNFOLD_THEME`**
    -   Type: `bool`
    -   Required: `False`
    -   Default: `False`
    -   Description: Whether to use the "unfold" theme for the admin panel.

---

## Branding

-   **`PLATFORM_NAME`**

    -   Type: `str`
    -   Required: `False`
    -   Default: `"Authentication Service"`
    -   Description: The name of the platform.

-   **`PLATFORM_ADDRESS`**

    -   Type: `str`
    -   Required: `False`
    -   Default: `"123 Main St."`
    -   Description: The physical address of the platform.

-   **`PLATFORM_CONTACT_EMAIL`**
    -   Type: `str`
    -   Required: `False`
    -   Default: `"support@waanverse.com"`
    -   Description: The contact email address for the platform.
