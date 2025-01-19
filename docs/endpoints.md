# API Routes Documentation

This document provides detailed information about the available API endpoints for authentication and user management.

## Authorization Endpoints

### Refresh Token

-   **URL:** `/refresh/`
-   **Method:** GET
-   **Description:** Refreshes the user's access token
-   **Name:** `dj_waanverse_auth_refresh_access_token`
-   **Authentication Required:** Yes

### Current User

-   **URL:** `/me/`
-   **Method:** GET
-   **Description:** Retrieves the currently authenticated user's information
-   **Name:** `dj_waanverse_auth_authenticated_user`
-   **Authentication Required:** Yes

### Logout

-   **URL:** `/logout/`
-   **Method:** POST
-   **Description:** Logs out the current user and invalidates their session
-   **Name:** `dj_waanverse_auth_logout`
-   **Required Data:** None
-   **Authentication Required:** Yes

### Home Page

-   **URL:** `/home/`
-   **Method:** GET
-   **Description:** Returns the home page for authenticated users
-   **Name:** `dj_waanverse_auth_home_page`
-   **Authentication Required:** Yes

## Login Endpoints

### Login

-   **URL:** `/login/`
-   **Method:** POST
-   **Description:** Authenticates a user and creates a new session
-   **Name:** `dj_waanverse_auth_login`
-   **Required Data:**
    -   `login_field`: The user's email, username, or phone number (string)
    -   `password`: The user's password (string)
    -   `turnstile_token`: (Optional) Token for Turnstile CAPTCHA validation (string)
-   **Authentication Required:** No

## Signup Endpoints

### Signup

-   **URL:** `/signup/`
-   **Method:** POST
-   **Description:** Creates a new user account
-   **Name:** `dj_waanverse_auth_signup`
-   **Required Data:**
    -   `username`: The desired username (string)
    -   `email_address`: The user's email address (string)
    -   `password`: The user's password (string)
    -   `confirm_password`: The user's password confirmation (string)
-   **Authentication Required:** No

### Initiate Email Verification

-   **URL:** `/signup/email/initiate-verification/`
-   **Method:** POST
-   **Description:** Sends an email verification link to the user
-   **Name:** `dj_waanverse_auth_initiate_email_verification`
-   **Required Data:**
    -   `email_address`: The email address to verify (string)
-   **Authentication Required:** No

### Verify Email

-   **URL:** `/signup/email/verify/`
-   **Method:** POST
-   **Description:** Verifies the user's email address using the verification token
-   **Name:** `dj_waanverse_auth_verify_email`
-   **Required Data:**
    -   `code`: The email verification token (string)
    -   `email_address`: The email address to verify (string)
-   **Authentication Required:** No

## Multi-Factor Authentication (MFA) Endpoints

### Get MFA Secret

-   **URL:** `/mfa/get-secret/`
-   **Method:** POST
-   **Description:** Generates and returns an MFA secret for the user
-   **Name:** `dj_waanverse_auth_get_mfa_secret`
-   **Authentication Required:** Yes

### Activate MFA

-   **URL:** `/mfa/activate/`
-   **Method:** POST
-   **Description:** Activates MFA for the user's account
-   **Name:** `dj_waanverse_auth_activate_mfa`
-   **Required Data:**
    -   `code`: The MFA code for activation (string)
-   **Authentication Required:** Yes

### Deactivate MFA

-   **URL:** `/mfa/deactivate/`
-   **Method:** POST
-   **Description:** Deactivates MFA for the user's account
-   **Name:** `dj_waanverse_auth_deactivate_mfa`
-   **Required Data:**
    -   `code`: The MFA code for deactivation (string)
    -   `password`: The user's password (string)
-   **Authentication Required:** Yes

### MFA Login

-   **URL:** `/mfa/login/`
-   **Method:** POST
-   **Description:** Handles the MFA step of the login process
-   **Name:** `dj_waanverse_auth_mfa_login`
-   **Required Data:**
    -   `code`: The MFA code for login (string)
    -   `user_id`: The user's unique identifier (string)
-   **Authentication Required:** Yes

### Get Recovery Codes

-   **URL:** `/mfa/recovery-codes/`
-   **Method:** GET
-   **Description:** Retrieves the user's MFA recovery codes
-   **Name:** `dj_waanverse_auth_get_recovery_codes`
-   **Authentication Required:** Yes

### Generate Recovery Codes

-   **URL:** `/mfa/generate-recovery-codes/`
-   **Method:** POST
-   **Description:** Generates new recovery codes for the user
-   **Name:** `dj_waanverse_auth_generate_recovery_codes`
-   **Required Data:** None
-   **Authentication Required:** Yes

## Password Management Endpoints

### Initiate Password Reset

-   **URL:** `/password/reset/`
-   **Method:** POST
-   **Description:** Initiates the password reset process by sending a reset link
-   **Name:** `dj_waanverse_auth_initiate_password_reset`
-   **Required Data:**
    -   `email_address`: The email address to send the reset link (string)
-   **Authentication Required:** No

### Reset Password

-   **URL:** `/password/new-password/`
-   **Method:** POST
-   **Description:** Allows users to set a new password using a reset token
-   **Name:** `dj_waanverse_auth_reset_password`
-   **Required Data:**
    -   `code`: The password reset token (string)
    -   `new_password`: The new password (string)
    -   `confirm_password`: The confirmation of the new password (string)
    -   `email_address`: The email address associated with the reset token (string)
-   **Authentication Required:** No

## Base URL Structure

All endpoints are organized under the following structure:

-   Authentication endpoints are at the root level
-   Login-related endpoints are under `/login/`
-   MFA-related endpoints are under `/mfa/`
-   Signup-related endpoints are under `/signup/`
-   Password management endpoints are under `/password/`
