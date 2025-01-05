# Quick Start Guide

This guide will help you get up and running with `dj_waanverse_auth` in minutes. For a more detailed setup, see our [Installation Guide](installation.md).

!!! important "Model Requirements"
    Your User model must include the following fields:

    - `username` (required): User's unique username
    - `email_address` (required): User's email address
    - `phone_number` (optional): User's phone number

    Example model configuration:
    ```python
    from django.contrib.auth.models import AbstractUser
    
    class User(AbstractUser):
        email_address = models.EmailField(unique=True)
        phone_number = models.CharField(max_length=15, blank=True, null=True)
        
        USERNAME_FIELD = 'username'
        REQUIRED_FIELDS = ['email_address']
    ```

## Prerequisites

Before starting, ensure you have:

- Python 3.11+
- Django 5.1+
- A Django project already set up

## 5-Minute Setup

### 1. Install the Package

```bash
pip install dj-waanverse-auth
```

### 2. Basic Configuration

Add these essential settings to your `settings.py`:

```python
# Add to INSTALLED_APPS
INSTALLED_APPS = [
    ...
    'dj_waanverse_auth',
]


# Configure Authentication
AUTHENTICATION_BACKENDS = [
    "dj_waanverse_auth.backends.AuthenticationBackend",
    "django.contrib.auth.backends.ModelBackend",
]


# Set up JWT Authentication
REST_FRAMEWORK = {
    "DEFAULT_AUTHENTICATION_CLASSES": (
        "dj_waanverse_auth.authentication.JWTAuthentication",
    ),
}


# Configure Email Settings (Required)
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.your-email-provider.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'your-email@example.com'
EMAIL_HOST_PASSWORD = 'your-email-password'
DEFAULT_FROM_EMAIL = 'your-email@example.com'


# Set up the Paths to both the public and private pem keys
WAANVERSE_AUTH_CONFIG = {
    "PUBLIC_KEY_PATH": "./secrets/public_key.pem",
    "PRIVATE_KEY_PATH": "./secrets/private_key.pem",
}
```

### 3. Verify Configuration

Run the auth-check command to verify your configuration and see which fields need to be set:

```bash
python manage.py auth-check
```

This command will:
- Check if all required settings are properly configured
- List any missing or incorrect WAANVERSE_AUTH_CONFIG fields
- Verify email settings are properly configured
- Suggest fixes for any issues found

### 4. Add URLs

In your `urls.py`:

```python
from django.urls import path, include

urlpatterns = [
    ...
    path('auth/', include('dj_waanverse_auth.urls')),
]
```

### 5. Run Migrations

```bash
python manage.py migrate
```

## Quick Test

Let's verify the setup with a simple authentication test:

1. Create a test user:
```bash
python manage.py createsuperuser
```

2. Make a login request:
```bash
curl -X POST http://localhost:8000/auth/login/ \
     -H "Content-Type: application/json" \
     -d '{"login_field": "your@email.com", "password": "yourpassword"}'
```

You should receive a JWT token response!

## Next Steps

- [Configure email settings](configuration.md#email-settings) for password reset functionality
- [Set up MFA](endpoints/mfa/overview.md) for enhanced security
- [Customize token settings](configuration.md#token-settings) for your needs

## Common Operations

### User Registration

```python
# Example API request
import requests

response = requests.post(
    'http://localhost:8000/auth/signup/',
    json={
        'email_address': 'user@example.com',
        "username": "username",
        'password': 'secure_password',
        'confirm_password': 'secure_password'
    }
)
```

### User Login

```python
response = requests.post(
    'http://localhost:8000/auth/login/',
    json={
        'email': 'user@example.com',
        'password': 'secure_password'
    }
)
```

### Protected Route Example

```python
# views.py
from dj_waanverse_auth.permissions import IsAuthenticated
from rest_framework.views import APIView

class ProtectedView(APIView):
    permission_classes = [IsAuthenticated]
    
    def get(self, request):
        return Response({"message": "Protected data"})
```

## Need Help?

- Check our [troubleshooting guide](advanced/troubleshooting.md)
- Join our [community forum](https://community.waanverse.com)
- Contact [support@waanverse.com](mailto:support@waanverse.com)