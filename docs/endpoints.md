# API Endpoints

`dj_waanverse_auth` provides authentication via **magic codes (email)** and **passkeys**. All endpoints return JSON responses.

---

## 1. Signup with Magic Code

### Step 1: Request Magic Code

-   **Frontend Path:** `/signup/`
-   **Django Name:** `dj_waanverse_auth_signup`
-   **Method:** `POST`

**Request Body:**

```json
{
    "email_address": "user@example.com"
}
```

**Description:**

-   Sends a one-time magic code to the email address.
-   User is inactive until verification.
-   Magic code is valid for 10 minutes.

---

### Step 2: Verify Code and Activate Account

-   **Frontend Path:** `/signup/`
-   **Django Name:** `dj_waanverse_auth_signup`
-   **Method:** `POST`

**Request Body:**

```json
{
    "email_address": "user@example.com",
    "code": "123456"
}
```

**Description:**

-   Verifies the magic code.
-   Creates and logs in the user.
-   Session is started, and `sid` must be stored by frontend.

---

## 2. Passkey Registration

### Step 1: Request Passkey Registration Options

-   **Frontend Path:** `/login/webauthn/options/`
-   **Django Name:** `dj_waanverse_auth_generate_registration_options`
-   **Method:** `POST`
-   **Requirements:** User must be logged in

---

### Step 2: Verify Passkey Registration

-   **Frontend Path:** `/login/webauthn/verify/`
-   **Django Name:** `dj_waanverse_auth_verify_registration`
-   **Method:** `POST`

**Request Body:**

```json
{
    "id": "credential_id",
    "rawId": "raw_credential_id",
    "type": "public-key",
    "response": {
        "attestationObject": "...",
        "clientDataJSON": "..."
    },
    "challengeId": "challenge_uuid",
    "name": "My Device"
}
```

---

## 3. Login

### Magic Code Login

#### Step 1: Request Login Code

-   **Frontend Path:** `/login/`
-   **Django Name:** `dj_waanverse_auth_login`
-   **Method:** `POST`
-   **Description:** Magic code sent to email, valid for 10 minutes.

**Request Body:**

```json
{
    "email_address": "user@example.com"
}
```

---

#### Step 2: Verify Magic Code

-   **Frontend Path:** `/login/`
-   **Django Name:** `dj_waanverse_auth_login`
-   **Method:** `POST`

**Request Body:**

```json
{
    "email_address": "user@example.com",
    "code": "123456"
}
```

---

### Passkey Login

#### Step 1: Request Passkey Challenge

-   **Frontend Path:** `/login/webauthn/`
-   **Django Name:** `dj_waanverse_auth_generate_authentication_options`
-   **Method:** `POST`

---

#### Step 2: Verify Passkey Login

-   **Frontend Path:** `/login/webauthn/verify-challenge/`
-   **Django Name:** `dj_waanverse_auth_verify_authentication`
-   **Method:** `POST`

**Request Body:**

```json
{
    "id": "credential_id",
    "rawId": "raw_credential_id",
    "type": "public-key",
    "response": {
        "authenticatorData": "...",
        "clientDataJSON": "...",
        "signature": "...",
        "userHandle": "..."
    }
}
```

---

## 4. Logout

### Step 1: Logout Active Session

-   **Frontend Path:** `/logout/`
-   **Django Name:** `dj_waanverse_auth_logout`
-   **Method:** `POST`

**Request Body:**

```json
{
    "access_token": "user_access_token"
}
```

**Note:** Token can also be retrieved from cookie.

---

### Step 2: Delete Expired Session

-   **Frontend Path:** `/sessions/<session_id>/`
-   **Django Name:** `dj_waanverse_auth_delete_user_session`
-   **Method:** `DELETE`

**Request Body:**

```json
{
    "sid": "abc123sessionid"
}
```

---

### Get User Sessions

-   **Frontend Path:** `/sessions/`
-   **Django Name:** `dj_waanverse_auth_get_user_sessions`
-   **Method:** `GET`

---

### Refresh Access Token

-   **Frontend Path:** `/refresh/`
-   **Django Name:** `dj_waanverse_auth_refresh_access_token`
-   **Method:** `POST`

**Request Body:**

```json
{
    "refresh_token": "user_refresh_token"
}
```

**Note:** Token can also be retrieved from cookie.

---

### Get Authenticated User

-   **Frontend Path:** `/me/`
-   **Django Name:** `dj_waanverse_auth_authenticated_user`
-   **Method:** `GET`

---
