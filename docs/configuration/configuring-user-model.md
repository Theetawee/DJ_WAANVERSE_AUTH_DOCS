# User Model Configuration

This document details the implementation and usage of the `AbstractBaseAccount` custom user model. This model provides a flexible authentication system supporting both email and phone number as contact methods, while using a username as the primary identifier.

## Key Features

- **Username-based authentication** with support for email and phone number.
- **Conditional unique constraints** for email and phone number.
- **Verification status tracking** for both email and phone.
- Extensible abstract base class.
- Custom model manager for user creation.
- Password management tracking.
- Comprehensive indexing for optimized query performance.

## Attributes and Methods

### Model Attributes

1. **`username`**
    - Type: `CharField`
    - Description: Unique primary identifier for the user (`USERNAME_MAX_LENGTH` characters max).

2. **`email_address`**
    - Type: `EmailField`
    - Description: User's email address; **required and unique**.

3. **`phone_number`**
    - Type: `CharField`
    - Description: User's phone number in E.164 format; optional but unique when provided.

4. **`date_joined`**
    - Type: `DateTimeField`
    - Description: Timestamp of user creation.
    - Default: Auto-set at creation.

5. **`last_login`**
    - Type: `DateTimeField`
    - Description: Timestamp of the last user login.
    - Default: Null/Blank.

6. **`is_active`**
    - Type: `BooleanField`
    - Description: Indicates if the user is active.
    - Default: True.

7. **`is_staff`**
    - Type: `BooleanField`
    - Description: Indicates if the user has staff permissions.
    - Default: False.

8. **`password_last_updated`**
    - Type: `DateTimeField`
    - Description: Tracks the last password change.
    - Default: Current timestamp.

9. **`email_verified`**
    - Type: `BooleanField`
    - Description: Indicates if the email address has been verified.
    - Default: False.

10. **`phone_number_verified`**
    - Type: `BooleanField`
    - Description: Indicates if the phone number has been verified.
    - Default: False.

### Model Methods

1. **`__str__()`**
    - Returns: Primary contact method (email or phone) or username.

2. **`get_full_name()`**
    - Returns: Full name of the user (inherited models can customize this).

3. **`get_short_name()`**
    - Returns: A short name for the user, usually the username.

4. **`get_primary_contact` (Property)**
    - Returns: The primary contact method (email if available, else phone).

5. **`has_perm(perm, obj=None)`**
    - Returns: Boolean indicating whether the user has a specific permission.
    - Default: True for staff users.

6. **`has_module_perms(app_label)`**
    - Returns: Boolean indicating whether the user has permissions for an app.
    - Default: True.

### Manager Methods

1. **`create_user(username, email_address, password=None, **extra_fields)`**
    - Creates and saves a regular user.
    - Validates the presence of `username` and **requires email_address**.
    - Returns: A user instance.

2. **`create_superuser(username, email_address, password, **extra_fields)`**
    - Creates and saves a superuser with all permissions.
    - Returns: A superuser instance.

## Implementation

### Basic Usage

To implement the custom user model, create a concrete model that inherits from `AbstractBaseAccount`:

```python
from django.db import models
from path.to.abstract_model import AbstractBaseAccount

class User(AbstractBaseAccount):
    # Add your custom fields here
    first_name = models.CharField(max_length=30, blank=True)
    last_name = models.CharField(max_length=30, blank=True)

    class Meta(AbstractBaseAccount.Meta):
        db_table = 'auth_user'
        swappable = 'AUTH_USER_MODEL'
```

### Configuration

In your Django settings:

```python
AUTH_USER_MODEL = 'yourapp.User'
```

## Extending the Model

### Adding Custom Fields

You can add additional fields to your concrete model:

```python
class User(AbstractBaseAccount):
    birth_date = models.DateField(null=True, blank=True)
    address = models.TextField(blank=True)
    profile_photo = models.ImageField(upload_to='profiles/', null=True, blank=True)
```

### Customizing the Meta Class

The Meta class can be extended while maintaining the base constraints:

```python
class User(AbstractBaseAccount):
    class Meta(AbstractBaseAccount.Meta):
        db_table = 'users'
        verbose_name = 'User'
        verbose_name_plural = 'Users'
        ordering = ['-date_joined']
        permissions = [
            ("can_view_profiles", "Can view user profiles"),
            ("can_edit_profiles", "Can edit user profiles"),
        ]
```

### Adding Custom Methods

Extend functionality by adding custom methods:

```python
class User(AbstractBaseAccount):
    def get_full_name(self):
        return f"{self.first_name} {self.last_name}".strip()

    def send_verification_email(self):
        # Implementation for sending verification email
        pass

    def send_verification_sms(self):
        # Implementation for sending verification SMS
        pass
```

## Model Manager Usage

### Creating Regular Users

```python
# Create user with email
user = User.objects.create_user(
    username='johndoe',
    password='secure_password',
    email_address='john@example.com'
)

# Create user with phone
user = User.objects.create_user(
    username='janedoe',
    password='secure_password',
    phone_number='+1234567890'
)
```

### Creating Superusers

```python
superuser = User.objects.create_superuser(
    username='admin',
    password='admin_password',
    email_address='admin@example.com'
)
```

## Validation

The model includes several built-in validations:

1. Username is required and must be unique.
2. **Email address is required** and must be unique.
3. Either email or phone number must be provided.
4. Phone number must be unique when provided.
5. Custom validations can be added in the concrete model.

## Model Constraints

### Built-in Constraints

The model includes conditional unique constraints for email and phone number, ensuring that **email address is mandatory and unique**. These constraints are automatically inherited by concrete models.

### Adding Custom Constraints

```python
class User(AbstractBaseAccount):
    class Meta(AbstractBaseAccount.Meta):
        constraints = [
            *AbstractBaseAccount.Meta.constraints,
            models.CheckConstraint(
                check=models.Q(age__gte=18),
                name='%(app_label)s_%(class)s_adult_age'
            )
        ]
```

## Performance Considerations

- Optimized indexes for username, email, and phone number lookups.
- Additional indexes can be added based on your specific query patterns.
- The `get_primary_contact` method is implemented as a property for better performance.

## Security Features

- Password updates are tracked via `password_last_updated`.
- Separate verification statuses for email and phone number.
- Built-in support for Django's permission system.
- Inactive user handling via `is_active` field.

## Best Practices

1. Always inherit from `AbstractBaseAccount` for custom user models.
2. Maintain the unique constraints when extending.
3. Use the provided manager methods for user creation.
4. Implement custom clean methods in concrete models when adding validation.
5. Add appropriate indexes for any additional fields used in lookups.

## Note on Migrations

When extending this model, remember to:

1. Make migrations after adding new fields or constraints.
2. Review generated migrations for correct constraint and index names.
3. Handle existing data appropriately when adding new required fields.

---

This update ensures the email address is required for user creation, aligning with your changes.