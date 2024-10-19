# **Messages Documentation**

This document provides a breakdown of the messages used throughout the authentication and multi-factor authentication (MFA) processes in your system. The messages are translated using Django’s `gettext_lazy` for i18n (internationalization) support.

---

## **Authentication & General Status Messages**

| Key                        | Message                                                                                 |
|----------------------------|-----------------------------------------------------------------------------------------|
| `status_unverified`         | "Your email remains unverified. Please verify it to proceed."                           |
| `status_verified`           | "Your email has been successfully verified."                                            |
| `no_account`                | "No active account found."                                                             |
| `email_sent`                | "A verification email has been dispatched to your email address."                      |
| `email_required`            | "A valid email address is required to continue."                                        |
| `invalid_account`           | "The provided account details are invalid. Please try again."                          |
| `already_authenticated`     | "You are already authenticated."                                                       |
| `logout_successful`         | "You have successfully logged out."                                                    |
| `general_msg`               | "An error occurred. Please try again later."                                           |
| `no_credentials`            | "No valid credentials provided. Please check and try again."                           |
| `user_creation_error`       | "An error occurred during user creation. Please try again."                            |

---

## **Password Reset Messages**

| Key                                    | Message                                                                                                         |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| `password_reset_code_sent`             | "A password reset code has been sent to your email address if an account associated to it is present."          |
| `password_reset_successful`            | "Your password has been reset successfully."                                                                   |
| `password_reset_attempts_limit`        | "Too many attempts. Please try again after the {COOLDOWN_PERIOD} minutes."                                      |
| `invalid_password`                     | "The password provided is invalid."                                                                            |
| `password_mismatch`                    | "The provided passwords do not match. Please try again."                                                       |
| `invalid_code`                         | "The provided code is invalid. Please try again."                                                              |
| `expired_code`                         | "The provided code has expired. Please try again."                                                             |
| `email_not_found`                      | "A password reset code has been sent to your email address if an account associated to it is present."          |

---

## **MFA (Multi-Factor Authentication) Messages**

| Key                                    | Message                                                                                                         |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| `mfa_enabled_success`                  | "Multi-factor authentication has been successfully enabled."                                                   |
| `mfa_already_activated`                | "Multi-factor authentication is already activated."                                                            |
| `mfa_invalid_otp`                      | "The provided OTP is invalid. Please try again."                                                               |
| `mfa_not_activated`                    | "Multi-factor authentication is not activated for this account."                                                |
| `mfa_deactivated`                      | "Multi-factor authentication has been successfully deactivated."                                               |
| `mfa_login_failed`                     | "MFA login failed due to an invalid OTP or recovery code."                                                     |
| `mfa_login_success`                    | "MFA login was successful."                                                                                    |
| `mfa_required`                         | "Multi-factor authentication is required for this action."                                                     |
| `mfa_recovery_codes_generated`         | "New MFA recovery codes have been successfully generated."                                                     |
| `mfa_setup_failed`                     | "An error occurred during the setup of multi-factor authentication."                                           |
| `recovery_codes_regeneration_error`    | "An error occurred while generating recovery codes. Please try again."                                         |

---

## **Email Subjects**

| Key                                    | Message                                                                                                         |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| `verify_email_subject`                 | "Verify your email address"                                                                                     |
| `login_email_subject`                  | "New login alert"                                                                                               |
| `mfa_code_generated_email_subject`     | "New Multi-factor Authentication Recovery Codes Generated"                                                     |
| `mfa_deactivated_email_subject`        | "Multi-factor Authentication Deactivation Confirmation"                                                        |
| `reset_password_email_subject`         | "Password Reset Request"                                                                                        |
| `password_reset_complete_email_subject`| "Password Reset Successful"                                                                                     |

---

## **Account and Registration Errors**

| Key                        | Message                                                                                 |
|----------------------------|-----------------------------------------------------------------------------------------|
| `email_exists`              | "An account with this email already exists."                                            |
| `username_exists`           | "An account with this username already exists."                                         |
| `email_already_verified`    | "Your email is already verified."                                                       |

---

## **Security and Token Errors**

| Key                        | Message                                                                                 |
|----------------------------|-----------------------------------------------------------------------------------------|
| `token_error`               | "An error occurred while processing the token. Please try again."                       |

---

### **Note:**
The cooldown period mentioned in `password_reset_attempts_limit` is dynamically filled from the `accounts_config.PASSWORD_RESET_COOLDOWN_PERIOD` setting.
