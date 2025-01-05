# Troubleshooting Guide

This guide covers common issues you might encounter while using `dj_waanverse_auth` and their solutions.

## Common Issues

### Authentication Errors

#### JWT Token Invalid or Expired

**Symptom:** Receiving `{"detail": "Invalid token"}` or `{"detail": "Token has expired"}` errors.

**Solutions:**
1. Check your token expiration settings in `WAANVERSE_AUTH_CONFIG`:
   ```python
   WAANVERSE_AUTH_CONFIG = {
       "TOKEN_LIFETIME": 3600,  # Default is 1 hour
       "REFRESH_TOKEN_LIFETIME": 86400,  # Default is 24 hours
   }
   ```
2. Verify your clock settings are correct on both client and server
3. Ensure you're using the correct public/private key pair

#### Unable to Login

**Symptom:** Login attempts fail with `{"detail": "Invalid credentials"}`.

**Solutions:**
1. Verify the user exists in the database
2. Check if the user is active (`is_active=True`)
3. Ensure the password was correctly hashed during user creation
4. Verify email verification status if required

```python
# Check user status in Django shell
from django.contrib.auth import get_user_model
User = get_user_model()
user = User.objects.get(email_address='user@example.com')
print(f"Active: {user.is_active}")
print(f"Email verified: {user.email_verified}")
```

### Configuration Issues

#### Missing Required Settings

**Symptom:** Server fails to start with configuration errors.

**Solution:** Run the configuration check:
```bash
python manage.py auth-check
```

Then verify all required settings are present:
```python
WAANVERSE_AUTH_CONFIG = {
    "PUBLIC_KEY_PATH": "./secrets/public_key.pem",
    "PRIVATE_KEY_PATH": "./secrets/private_key.pem",
    "TOKEN_LIFETIME": 3600,
    "REFRESH_TOKEN_LIFETIME": 86400,
    "PASSWORD_RESET_TIMEOUT": 3600,
}
```

#### Email Configuration Issues

**Symptom:** Password reset or verification emails not being sent.

**Solutions:**
1. Verify email settings:
   ```python
   EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
   EMAIL_HOST = 'smtp.your-email-provider.com'
   EMAIL_PORT = 587
   EMAIL_USE_TLS = True
   EMAIL_HOST_USER = 'your-email@example.com'
   EMAIL_HOST_PASSWORD = 'your-email-password'
   DEFAULT_FROM_EMAIL = 'your-email@example.com'
   ```

2. Test email configuration:
   ```python
   from django.core.mail import send_mail
   
   send_mail(
       'Test Subject',
       'Test Message',
       'from@example.com',
       ['to@example.com'],
       fail_silently=False,
   )
   ```

### Database Issues

#### Migration Problems

**Symptom:** Migration errors when running `python manage.py migrate`.

**Solutions:**
1. Ensure all dependencies are installed:
   ```bash
   pip install -r requirements.txt
   ```

2. Reset migrations if necessary:
   ```bash
   python manage.py migrate dj_waanverse_auth zero
   python manage.py migrate dj_waanverse_auth
   ```

3. Check for conflicting migrations:
   ```bash
   python manage.py showmigrations dj_waanverse_auth
   ```

### Permission Issues

#### Protected Routes Not Working

**Symptom:** Receiving `{"detail": "Authentication credentials were not provided"}`.

**Solutions:**
1. Verify authentication classes:
   ```python
   REST_FRAMEWORK = {
       "DEFAULT_AUTHENTICATION_CLASSES": (
           "dj_waanverse_auth.authentication.JWTAuthentication",
       ),
   }
   ```

2. Check request headers:
   ```python
   headers = {
       'Authorization': f'Bearer {token}'
   }
   ```

3. Verify permission classes:
   ```python
   from dj_waanverse_auth.permissions import IsAuthenticated
   
   class YourView(APIView):
       permission_classes = [IsAuthenticated]
   ```

## Debug Logging

Enable detailed logging to track authentication issues:

```python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
        },
        'file': {
            'class': 'logging.FileHandler',
            'filename': 'debug.log',
        },
    },
    'loggers': {
        'dj_waanverse_auth': {
            'handlers': ['console', 'file'],
            'level': 'DEBUG',
        },
    },
}
```

## Custom User Model Issues

#### Fields Missing or Incorrect

**Symptom:** User creation fails or authentication doesn't work properly.

**Solution:** Verify your custom user model has all required fields:
```python
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    email_address = models.EmailField(unique=True)
    phone_number = models.CharField(max_length=15, blank=True, null=True)
    
    USERNAME_FIELD = 'username'
    REQUIRED_FIELDS = ['email_address']
```

## Performance Issues

### Slow Authentication

**Symptom:** Authentication requests taking longer than expected.

**Solutions:**
1. Enable caching for token verification:
   ```python
   WAANVERSE_AUTH_CONFIG = {
       "ENABLE_TOKEN_CACHE": True,
       "TOKEN_CACHE_TIMEOUT": 300,  # 5 minutes
   }
   ```

2. Monitor database queries:
   ```python
   from django.db import connection
   
   # After your view logic
   print(len(connection.queries))  # Number of queries
   for query in connection.queries:
       print(query['sql'])  # SQL statements
   ```

## Still Having Issues?

If you're still experiencing problems:

1. Check our [GitHub Issues](https://github.com/waanverse/dj-waanverse-auth/issues) for similar problems
2. Enable debug mode temporarily:
   ```python
   DEBUG = True
   ```
3. Contact support with:
   - Your Django version
   - Your dj_waanverse_auth version
   - Relevant error messages
   - Steps to reproduce the issue

## Support Resources

- Documentation: [https://docs.waanverse.com/dj-waanverse-auth](https://docs.waanverse.com/dj-waanverse-auth)
- Community Forum: [https://community.waanverse.com](https://community.waanverse.com)
- Email Support: [support@waanverse.com](mailto:support@waanverse.com)