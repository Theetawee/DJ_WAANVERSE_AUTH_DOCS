# Dj Waanverse Auth

[![PyPI version](https://badge.fury.io/py/dj-waanverse-auth.svg)](https://badge.fury.io/py/dj-waanverse-auth)
[![License](https://img.shields.io/badge/license-Proprietary-blue.svg)](https://www.waanverse.com/licenses)
[![Python](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Django](https://img.shields.io/badge/django-5.0+-green.svg)](https://www.djangoproject.com/)

## Overview

`dj_waanverse_auth` is a Django authentication package designed for modern applications, providing **passwordless authentication** using:

-   **Magic login codes via email**
-   **Passkeys (WebAuthn)**

It simplifies user authentication while maintaining enterprise-grade security.

## Installation

Install the package via pip:

```bash
pip install dj-waanverse-auth
```

Add it to your `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    ...
    "dj_waanverse_auth",
]
```

Add the middleware:

```python
MIDDLEWARE = [
    ...
    "dj_waanverse_auth.middleware.auth.AuthCookieMiddleware",
]
```

## Authentication Backends

Configure Django to use the custom authentication backends:

```python
AUTHENTICATION_BACKENDS = [
    "django.contrib.auth.backends.ModelBackend",
    "dj_waanverse_auth.backends.AuthenticationBackend",
]
```

## Django REST Framework Configuration

Set the default authentication classes:

```python
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": (
        "dj_waanverse_auth.authentication.JWTAuthentication",
    ),
}
```

## Waanverse Auth Config

Add Waanverse-specific configuration:

```python

WAANVERSE_AUTH_CONFIG = {
    "PLATFORM_NAME": "My Platform",
    "BASIC_ACCOUNT_SERIALIZER": "path.to.BasicAccountSerializer",
    "PUBLIC_KEY_PATH": "path/to/public_key.pem",
    "PRIVATE_KEY_PATH": "path/to/private_key.pem",
    "WEBAUTHN_DOMAIN" = "example.com"
    "WEBAUTHN_RP_NAME" = "My App",
    "WEBAUTHN_ORIGIN" = "example.com",

}
```

More detailed configuration options are available in the [Configuration Guide](configuration/index.md).

## Email Backend (Required for Magic Codes)

```python
EMAIL_BACKEND = "django.core.mail.backends.smtp.EmailBackend"
EMAIL_HOST = "smtp.example.com"
EMAIL_PORT = 587
EMAIL_HOST_USER = "your-email@example.com"
EMAIL_HOST_PASSWORD = "your-password"
EMAIL_USE_TLS = True
DEFAULT_FROM_EMAIL = "noreply@example.com"
```

## Getting Started

After installation and configuration:

1. Users can log in using **magic codes** sent to their email.
2. Users can register and authenticate with **passkeys** for stronger security.
---

_Built with ❤️ by Waanverse Labs Inc._

```
