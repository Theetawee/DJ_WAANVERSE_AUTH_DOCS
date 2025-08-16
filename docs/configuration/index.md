# Authentication Configuration

`dj_waanverse_auth` provides a highly configurable authentication system. All configuration options are set via the `WAANVERSE_AUTH_CONFIG` dictionary in your Django `settings.py`.

The configuration is validated and stored in the `AuthConfig` class, with sensible defaults applied when values are not provided.

---

## Basic Settings

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `PLATFORM_NAME` | str | None | Name of your platform, e.g., `"Waanverse"` |
| `BASIC_ACCOUNT_SERIALIZER` | str | `"dj_waanverse_auth.serializers.base_serializers.BasicAccountSerializer"` | Serializer class used to expose basic account information |
| `PUBLIC_KEY_PATH` | str | None | Path to your JWT public key |
| `PRIVATE_KEY_PATH` | str | None | Path to your JWT private key |

---

## Cookie Configuration

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `ACCESS_TOKEN_COOKIE_NAME` | str | `"access_token"` | Name of the access token cookie |
| `REFRESH_TOKEN_COOKIE_NAME` | str | `"refresh_token"` | Name of the refresh token cookie |
| `COOKIE_PATH` | str | `"/"` | Path where the cookies are valid |
| `COOKIE_DOMAIN` | Optional[str] | None | Domain for the cookies |
| `COOKIE_SAMESITE_POLICY` | str | `"Lax"` | SameSite policy for cookies |
| `COOKIE_SECURE` | bool | `False` | Whether cookies require HTTPS |
| `COOKIE_HTTP_ONLY` | bool | `True` | Prevent JavaScript access to cookies |
| `ACCESS_TOKEN_COOKIE_MAX_AGE` | timedelta | 30 minutes | Expiration for access token cookie |
| `REFRESH_TOKEN_COOKIE_MAX_AGE` | timedelta | 30 days | Expiration for refresh token cookie |

---

## Email & Verification

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `BLACKLISTED_EMAILS` | List[str] | `[]` | Emails that cannot register |
| `BLACKLISTED_PHONE_NUMBERS` | List[str] | `[]` | Phone numbers that cannot register |
| `ALLOWED_EMAIL_DOMAINS` | List[str] | `[]` | Restrict registration to specific domains |
| `VERIFICATION_EMAIL_SUBJECT` | str | `"Verify your email address"` | Subject for verification emails |
| `LOGIN_CODE_EMAIL_SUBJECT` | str | `"Login code"` | Subject for magic code emails |
| `LOGIN_ALERT_EMAIL_SUBJECT` | str | `"Login alert"` | Subject for login alert notifications |

---

## WebAuthn / Passkeys

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `WEBAUTHN_DOMAIN` | str | None | Your domain for WebAuthn challenges, e.g., `"example.com"` |
| `WEBAUTHN_RP_NAME` | str | None | Name of your relying party for WebAuthn |
| `WEBAUTHN_ORIGIN` | str | None | The origin URL used to validate WebAuthn requests, e.g., `"https://example.com"` |

---

## Admin & Signup

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `ENABLE_ADMIN_PANEL` | bool | `False` | Whether to enable Django admin integration for authentication data |
| `DISABLE_SIGNUP` | bool | `False` | Disable user signup entirely |

---

## Reserved Usernames

```python
RESERVED_USERNAMES = ["admin", "administrator", "root", "system"]
```

---

## Example `settings.py` Usage

```python
import os
from datetime import timedelta

WAANVERSE_AUTH_CONFIG = {
    "PLATFORM_NAME": "Waanverse",
    "BASIC_ACCOUNT_SERIALIZER": "accounts.serializers.BasicAccountSerializer",
    "PUBLIC_KEY_PATH": os.path.join(BASE_DIR, "secrets/public_key.pem"),
    "PRIVATE_KEY_PATH": os.path.join(BASE_DIR, "secrets/private_key.pem"),
    "WEBAUTHN_DOMAIN": "example.com",
    "WEBAUTHN_RP_NAME": "My App",
    "WEBAUTHN_ORIGIN": "https://example.com",
    "ACCESS_TOKEN_COOKIE_MAX_AGE": timedelta(minutes=30),
    "REFRESH_TOKEN_COOKIE_MAX_AGE": timedelta(days=30),
}
```

> This configuration provides a full setup for magic code and passkey authentication. You can override any default to match your security and business requirements.

---

