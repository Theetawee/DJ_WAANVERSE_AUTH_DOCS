# Authentication Configuration Documentation

This document provides an explanation of the various settings available in the authentication configuration for the Waanverse authentication service.

---

## Security Settings

### `PUBLIC_KEY_PATH`

**Type**: str  
**Description**: The file path to the public key used for verifying tokens. Leave empty if not applicable.

### `PRIVATE_KEY_PATH`

**Type**: str  
**Description**: The file path to the private key used for signing tokens. Leave empty if not applicable.

### `HEADER_NAME`

**Type**: str  
**Default**: "X-Auth-Token"  
**Description**: The name of the HTTP header used for sending authentication tokens.

### `DEVICE_ID_HEADER_NAME`

**Type**: str  
**Default**: "X-Device-Id"  
**Description**: The name of the HTTP header used for identifying devices.

### `DEVICE_COOKIE_NAME`

**Type**: str  
**Default**: "device_id"  
**Description**: The name of the cookie used to store device identifiers.

### `USER_ID_CLAIM`

**Type**: str  
**Default**: "user_id"  
**Description**: The claim in the token that identifies the user.

### `DEVICE_AUTH_EXCLUDED_PATHS`

**Type**: List[str]  
**Default**: []  
**Description**: A list of paths excluded from device authentication checks.

---

## Cookie Configuration

### `ACCESS_TOKEN_COOKIE_NAME`

**Type**: str  
**Default**: "access_token"  
**Description**: The name of the cookie storing access tokens.

### `REFRESH_TOKEN_COOKIE_NAME`

**Type**: str  
**Default**: "refresh_token"  
**Description**: The name of the cookie storing refresh tokens.

### `COOKIE_PATH`

**Type**: str  
**Default**: "/"  
**Description**: The path for which cookies are valid.

### `COOKIE_DOMAIN`

**Type**: Optional[str]  
**Default**: None  
**Description**: The domain for which cookies are valid. Leave empty to apply cookies to all subdomains.

### `COOKIE_SAMESITE_POLICY`

**Type**: str  
**Default**: "Lax"  
**Description**: The SameSite policy for cookies. Valid options: "Strict", "Lax", "None".

### `COOKIE_SECURE`

**Type**: bool  
**Default**: False  
**Description**: Whether cookies should only be transmitted over HTTPS.

### `COOKIE_HTTP_ONLY`

**Type**: bool  
**Default**: True  
**Description**: Whether cookies should be inaccessible to JavaScript.

### `ACCESS_TOKEN_COOKIE_MAX_AGE`

**Type**: timedelta  
**Default**: 30 minutes  
**Description**: The maximum age of the access token cookie.

### `REFRESH_TOKEN_COOKIE_MAX_AGE`

**Type**: timedelta  
**Default**: 30 days  
**Description**: The maximum age of the refresh token cookie.

---

## Multi-Factor Authentication (MFA)

### `MFA_TOKEN_COOKIE_NAME`

**Type**: str  
**Default**: "mfa"  
**Description**: The name of the cookie storing MFA tokens.

### `MFA_TOKEN_COOKIE_MAX_AGE`

**Type**: timedelta  
**Default**: 2 minutes  
**Description**: The maximum age of the MFA token cookie.

### `MFA_RECOVERY_CODE_COUNT`

**Type**: int  
**Default**: 10  
**Description**: The number of recovery codes generated for MFA.

### `MFA_ISSUER_NAME`

**Type**: str  
**Default**: "Authentication Service"  
**Description**: The issuer name displayed in authentication apps.

### `MFA_CODE_LENGTH`

**Type**: int  
**Default**: 6  
**Description**: The length of the MFA code.

### `MFA_EMAIL_NOTIFICATIONS`

**Type**: bool  
**Default**: True  
**Description**: Whether email notifications are sent for MFA events.

### `MFA_CHANGED_EMAIL_SUBJECT`

**Type**: str  
**Default**: "Account security alert"  
**Description**: The subject of email notifications sent when MFA settings are changed.

---

## User Configuration

### `USERNAME_MIN_LENGTH`

**Type**: int  
**Default**: 4  
**Description**: The minimum length for usernames.

### `RESERVED_USERNAMES`

**Type**: List[str]  
**Default**: ["admin", "administrator", "root", "system"]  
**Description**: A list of reserved usernames that cannot be registered.

---

## Serializer Classes

### `BASIC_ACCOUNT_SERIALIZER`

**Type**: str  
**Default**: "dj_waanverse_auth.serializers.base_serializers.BasicAccountSerializer"  
**Description**: The serializer class for basic account information.

### `REGISTRATION_SERIALIZER`

**Type**: str  
**Default**: "dj_waanverse_auth.serializers.signup_serializers.SignupSerializer"  
**Description**: The serializer class for user registration.
---

## Email Settings
### `EMAIL_VERIFICATION_CODE_LENGTH`

**Type**: int  
**Default**: 6  
**Description**: The length of email verification codes.

### `EMAIL_VERIFICATION_CODE_IS_ALPHANUMERIC`

**Type**: bool  
**Default**: False  
**Description**: Whether email verification codes are alphanumeric.

### `EMAIL_SECURITY_NOTIFICATIONS_ENABLED`

**Type**: bool  
**Default**: True  
**Description**: Whether security notifications are sent via email.

### `EMAIL_THREADING_ENABLED`

**Type**: bool  
**Default**: True  
**Description**: Whether email operations use threading to improve performance.

### `BLACKLISTED_EMAILS`

**Type**: List[str]  
**Default**: []  
**Description**: A list of blacklisted email addresses.

### `DISPOSABLE_EMAIL_DOMAINS`

**Type**: List[str]  
**Default**: []  
**Description**: A list of disposable email domains that are not allowed.

### `EMAIL_BATCH_SIZE`

**Type**: int  
**Default**: 50  
**Description**: The batch size for email operations.

### `EMAIL_RETRY_ATTEMPTS`

**Type**: int  
**Default**: 3  
**Description**: The number of retry attempts for email delivery.

### `EMAIL_RETRY_DELAY`

**Type**: int  
**Default**: 5  
**Description**: The delay (in seconds) between email delivery retries.

### `EMAIL_MAX_RECIPIENTS`

**Type**: int  
**Default**: 50  
**Description**: The maximum number of recipients per email.

### `EMAIL_THREAD_POOL_SIZE`

**Type**: int  
**Default**: 5  
**Description**: The thread pool size for email operations.

### `VERIFICATION_EMAIL_SUBJECT`

**Type**: str  
**Default**: "Verify your email address"  
**Description**: The subject line for email verification messages.

### `VERIFICATION_EMAIL_CODE_EXPIRATION_TIME_MINUTES`

**Type**: int  
**Default**: 15  
**Description**: The expiration time for email verification codes (in minutes).

### `LOGIN_ALERT_EMAIL_SUBJECT`

**Type**: str  
**Default**: "New login alert"  
**Description**: The subject line for login alert emails.

### `SEND_LOGIN_ALERT_EMAILS`

**Type**: bool  
**Default**: False  
**Description**: Whether to send email alerts for new logins.

---

## Password Reset

### `PASSWORD_RESET_CODE_EXPIRY_IN_MINUTES`

**Type**: int  
**Default**: 10  
**Description**: The expiration time for password reset codes (in minutes).


### `PASSWORD_RESET_EMAIL_SUBJECT`

**Type**: str  
**Default**: "Password reset request"  
**Description**: The subject line for password reset emails.

---

## Admin Interface

### `ENABLE_ADMIN_PANEL`

**Type**: bool  
**Default**: False  
**Description**: Whether the admin panel is enabled.

### `USE_UNFOLD_THEME`

**Type**: bool  
**Default**: False  
**Description**: Whether to use the "unfold" theme for the admin panel.

---

## Branding

### `PLATFORM_NAME`

**Type**: str  
**Default**: "Authentication Service"  
**Description**: The name of the platform.

### `PLATFORM_ADDRESS`

**Type**: str  
**Default**: "123 Main St."  
**Description**: The physical address of the platform.

### `PLATFORM_CONTACT_EMAIL`

**Type**: str  
**Default**: "support@waanverse.com"  
**Description**: The contact email address for the platform.

---

This configuration provides a flexible and secure way to manage authentication settings. Modify the defaults as needed to suit your application's requirements.
