
# Configuring the User Model

`dj_waanverse_auth` uses a custom user model based on `AbstractBaseAccount` to provide flexible authentication using **email addresses** as the primary contact method. Passwords are only required for superusers, while regular users authenticate via magic codes (email) or passkeys.

---

## Key Features

-   **Email-based authentication** as primary identifier.
-   Passwords **required only for superusers**.
-   Automatic username generation if not provided.
-   User activation and email verification tracking.
-   Extensible abstract base model.
-   Custom manager for user and superuser creation.
-   Optimized indexing for queries.

---

## Model Attributes

| Attribute        | Type          | Default                        | Description                          |
| ---------------- | ------------- | ------------------------------ | ------------------------------------ |
| `username`       | CharField     | Auto-generated if not provided | Unique identifier; max 35 characters |
| `email_address`  | EmailField    | Required                       | Primary contact; must be unique      |
| `date_joined`    | DateTimeField | Auto-set                       | User creation timestamp              |
| `last_login`     | DateTimeField | Null/blank                     | Timestamp of last login              |
| `is_active`      | BooleanField  | `False`                        | User activation status               |
| `is_staff`       | BooleanField  | `False`                        | Staff permissions                    |
| `email_verified` | BooleanField  | `False`                        | Email verification status            |

---

## Model Methods

-   `__str__()` – Returns the user's email or username.
-   `get_full_name()` – Returns the user's email or username.
-   `get_short_name()` – Returns the user's email or username.
-   `has_perm(perm, obj=None)` – Returns `True` for staff users.
-   `has_module_perms(app_label)` – Always returns `True`.

---

## Manager Methods

### Creating Regular Users

```python
from yourapp.models import User

# Create a regular user (no password required)
user = User.objects.create_user(
    email_address="user@example.com"
)

print(user.username)  # Auto-generated if not provided
print(user.is_active)  # False by default
print(user.email_verified)  # False by default
```

**Notes:**

-   Email is required.
-   Users are inactive by default; must be verified before full access.
-   Password is not required; authentication uses magic codes or passkeys.

---

### Creating Superusers

```python
superuser = User.objects.create_superuser(
    email_address="admin@example.com",
    password="secure_superuser_password"
)

print(superuser.is_superuser)  # True
print(superuser.is_staff)      # True
```

**Notes:**

-   Password is required for superusers.
-   Email must be provided.
-   Superusers are active by default.

---

## Implementation Example

```python
from django.db import models
from path.to.abstract_model import AbstractBaseAccount

class User(AbstractBaseAccount):
    first_name = models.CharField(max_length=30, blank=True)
    last_name = models.CharField(max_length=30, blank=True)

    class Meta(AbstractBaseAccount.Meta):
        db_table = "auth_user"
        swappable = "AUTH_USER_MODEL"
```

Set the custom user model in your Django settings:

```python
AUTH_USER_MODEL = "yourapp.User"
```

---

## Extending the Model

-   **Add custom fields** like `birth_date`, `address`, or `profile_photo`.
-   **Custom methods** can be added, e.g., `send_verification_email()` or `send_verification_sms()`.
-   Extend the **Meta class** to include custom permissions or ordering:

```python
class User(AbstractBaseAccount):
    class Meta(AbstractBaseAccount.Meta):
        ordering = ["-date_joined"]
        permissions = [
            ("can_view_profiles", "Can view user profiles"),
        ]
```

---

## Validation

-   Email is **required** and **unique**.
-   Passwords are only required for superusers.
-   Users must be **active** and **email verified** to access protected features.

---

## Indexes and Constraints

-   Indexes on `username` and `email_address` for optimized queries.
-   Unique constraint enforced on `email_address`.
-   Additional constraints can be added via the concrete model’s Meta class.

---

## Best Practices

1. Inherit from `AbstractBaseAccount` for all user models.
2. Maintain unique constraints for email.
3. Use manager methods for creating users and superusers.
4. Implement verification workflows for email addresses.
5. Ensure `is_active` and `email_verified` are checked before allowing login.

