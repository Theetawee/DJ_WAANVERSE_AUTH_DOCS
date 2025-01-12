# Getting Started with dj-waanverse-auth

Welcome to **dj-waanverse-auth**, a library designed to streamline authentication and device management for your Django projects. Follow this guide to install and configure the package in your project.

---

## Installation

Install the package via pip:

```bash
pip install dj-waanverse-auth
```

---

## Setup

### 1. Add to Installed Apps

In your `settings.py`, include `dj_waanverse_auth` in the `INSTALLED_APPS` list:

```python
INSTALLED_APPS = [
    # Django default apps
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    # Third-party and custom apps
    "dj_waanverse_auth",
    "rest_framework",
    "accounts",  # Replace with your custom user app
]
```

---

### 2. Add Middleware

Include the `DeviceAuthMiddleware` in your `MIDDLEWARE` settings:

```python
MIDDLEWARE = [
    ...
    "dj_waanverse_auth.middleware.DeviceAuthMiddleware",
]
```

---

### 3. Set Custom User Model

Set up the user model as described in [Configuring User Model](/configuration/configuring-user-model).

Specify your custom user model in `settings.py`:

```python
AUTH_USER_MODEL = "accounts.Account"  # Replace `accounts.Account` with your custom user model
```

---

### 4. Configure Authentication Backends

Add the custom authentication backend to enable JWT-based authentication:

```python
AUTHENTICATION_BACKENDS = [
    "dj_waanverse_auth.backends.AuthenticationBackend",  # Custom backend
    "django.contrib.auth.backends.ModelBackend",         # Default Django backend
]
```

---

### 5. Configure REST Framework

Set up the `DEFAULT_AUTHENTICATION_CLASSES` for Django REST Framework in `settings.py`:

```python
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": (
        "dj_waanverse_auth.authentication.JWTAuthentication",
    ),
    "TEST_REQUEST_DEFAULT_FORMAT": "json",
}
```

---

### 6. Add Private/Public Key Paths

Add the `PUBLIC_KEY_PATH` and `PRIVATE_KEY_PATH` to the package's settings configuration in your `settings.py`:

```python
WAANVERSE_AUTH_CONFIG = {
    "PUBLIC_KEY_PATH": "/path/to/public_key.pem",
    "PRIVATE_KEY_PATH": "/path/to/private_key.pem",
}
```

For a complete list of configurable settings, check out [Configuration](/configuration).

---

### 7. Configure URLs

Include the package's URLs in your project's URL configuration:

```python
from django.urls import path, include

urlpatterns = [
    ...
    path("api/v1/", include("dj_waanverse_auth.urls")),
]
```

---

## Final Steps

### 1. Apply Migrations

Run the following command to apply migrations:

```bash
python manage.py migrate
```

---

### 2. Start the Development Server

Run the server to ensure everything is configured correctly:

```bash
python manage.py runserver
```

---

### 3. Run Configuration Check

You can verify your setup and identify any missing required settings using the following command:

```bash
python manage.py auth-check
```

This will provide feedback on your configuration and highlight any missing settings or issues.

---

🎉 **Congratulations!** Your Django project is now set up to use **dj-waanverse-auth** for advanced authentication and device management.

