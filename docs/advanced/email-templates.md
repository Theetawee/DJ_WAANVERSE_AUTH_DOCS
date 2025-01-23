### Email Service Documentation

The `EmailService` class handles the sending of various types of emails for user interaction and security purposes. This documentation explains what emails are sent, the context available in each email, and how to override the templates.

---

### Emails Sent by the Service

#### 1. **Verify Email Address**
- **Description**: This email is sent to verify the user's email address during sign-up or account creation.
- **Context Passed**:
  - `code`: The verification code to validate the email address.
  - `site_name`: The platform name.
  - `company_address`: The platform's physical address.
  - `support_email`: The platform's support email address.
- **Template**:
  - Default location: `emails/verify_email.html`
  - **Override**: Create a base template folder in your root project, add the `emails` folder, and add the `verify_email.html` file.

---

#### 2. **Login Email**
- **Description**: Sent each time a user logs into their account to alert them of the login activity.
- **Context Passed**:
  - `user`: The user object.
  - `ip_address`: The IP address of the login attempt.
  - `device_info`: Information about the device used for login.
  - `location_info`: The approximate location of the login attempt (based on IP address).
  - `timestamp`: The timestamp of the login.
  - `site_name`: The platform name.
  - `company_address`: The platform's physical address.
  - `support_email`: The platform's support email address.
- **Template**:
  - Default location: `emails/login_alert.html`
  - **Override**: Create a base template folder in your root project, add the `emails` folder, and add the `login_alert.html` file.

---

#### 3. **Password Reset Email**
- **Description**: Sent when a user requests to reset their password.
- **Context Passed**:
  - `code`: The password reset token.
  - `expiry_time`: The time before the reset token expires ({time} minutes).
  - `site_name`: The platform name.
  - `company_address`: The platform's physical address.
  - `support_email`: The platform's support email address.
- **Template**:
  - Default location: `emails/password_reset.html`
  - **Override**: Create a base template folder in your root project, add the `emails` folder, and add the `password_reset.html` file.

---

#### 4. **Account Locked Notification**
- **Description**: Sent when a user's account is locked due to multiple failed login attempts.
- **Context Passed**:
  - `support_email`: The platform's support email address.
  - `locked_time`: The timestamp when the account was locked.
  - `site_name`: The platform name.
  - `company_address`: The platform's physical address.
- **Template**:
  - Default location: `emails/account_locked.html`
  - **Override**: Create a base template folder in your root project, add the `emails` folder, and add the `account_locked.html` file.

---

#### 5. **MFA Change Notification**
- **Description**: Sent when Multi-Factor Authentication (MFA) is enabled or disabled.
- **Context Passed**:
  - `mfa_time`: The timestamp when MFA was enabled or disabled.
  - `site_name`: The platform name.
  - `company_address`: The platform's physical address.
  - `support_email`: The platform's support email address.
- **Template**:
  - Default location: `emails/mfa_enabled.html` (for enabling MFA) or `emails/mfa_disabled.html` (for disabling MFA).
  - **Override**: Create a base template folder in your root project, add the `emails` folder, and add the respective template files.

---

### How to Override Email Templates
To customize the email templates:
1. Create a base template folder in your root project.
2. Inside the base folder, create a folder named `emails`.
3. Add the specific email template file(s) you wish to override, following the naming convention mentioned above (e.g., `verify_email.html`, `login_alert.html`).
4. Customize the templates as needed.

Templates must include placeholders for the context variables passed by the service. For example, the `verify_email.html` template should include `{{ code }}` to display the verification code.

---

### Security Notifications
If `EMAIL_SECURITY_NOTIFICATIONS_ENABLED` is set to `True` in the settings, the following security-related emails are sent:
- **Verify Email Address**: To confirm email ownership during sign-up.
- **Login Email**: Alerts users of new login attempts.
- **Account Locked Notification**: Notifies users when their account is locked.
- **MFA Change Notification**: Informs users when MFA settings are modified.

Ensure the templates for these emails are properly overridden if customization is needed.

