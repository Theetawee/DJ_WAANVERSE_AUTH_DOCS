# Email Service Documentation

The `EmailService` class is responsible for sending emails used during authentication and login processes. This documentation explains which emails are sent, what context is available, and how to override the templates.

---

## Emails Sent by the Service

### 1. **Verify Email Address**

-   **Description**: Sent during sign-up to verify the user’s email address.
-   **Context Passed**:

    -   `code`: The verification code.
    -   `user`: The user object.

-   **Template**:

    -   Default: `emails/verify_email.html`
    -   **Override**: Create `emails/verify_email.html` inside your project’s base templates folder.

---

### 2. **Login Code**

-   **Description**: Sent when a user attempts to log in with their email. Contains the one-time login code.
-   **Context Passed**:

    -   `code`: The login code.
    -   `user`: The user object.

-   **Template**:

    -   Default: `emails/login_code.html`
    -   **Override**: Create `emails/login_code.html` inside your project’s base templates folder.

---

### 3. **Login Alert**

-   **Description**: Sent after a successful login to notify the user about the activity.
-   **Context Passed**:

    -   `user`: The user object.
    -   `ip_address`: IP address of the login attempt.
    -   `device`: Information about the device.
    -   `location`: Approximate location (from IP).

-   **Template**:

    -   Default: `emails/login_alert.html`
    -   **Override**: Create `emails/login_alert.html` inside your project’s base templates folder.

---

## How to Override Templates

1. Create a `templates` folder in your root project (if not already present).
2. Inside it, create an `emails` folder.
3. Add the template file you want to override (`verify_email.html`, `login_code.html`, `login_alert.html`).
4. Use the context variables listed above inside your template with `{{ variable_name }}`.

Example for `verify_email.html`:

```html
<p>Welcome to {{ site_name }}!</p>
<p>Your verification code is: <strong>{{ code }}</strong></p>
<p>If you didn’t request this, please contact {{ support_email }}.</p>
```

---
