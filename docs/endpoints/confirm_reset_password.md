## **Password Reset Confirmation Endpoint**

- **Route:** `POST /password/reset/new`
- **Description:** Completes the password reset process by verifying the reset code and updating the user’s password. An email confirming the password change is also sent to the user.

---

### **Request Details**

- **Method:** `POST`
- **Content-Type:** `application/json`

### **Request Body:**

```json
{
    "email": "string",
    "code": "string",
    "new_password1": "string",
    "new_password2": "string"
}
```

- **Required Fields:**
    - **email**: The email address associated with the account.
    - **code**: The reset code sent to the user's email.
    - **new_password1**: The new password the user wants to set.
    - **new_password2**: Confirmation of the new password (must match `new_password1`).

---

### **Successful Response**

- When the reset code is valid, and the new password is successfully set.
- **Status Code:** `200 OK`
- **Response Content:**

```json
{
    "msg": "[Messages.password_reset_successful]*",
}
```

- **msg**: Confirmation message indicating the password has been successfully reset.

---

### **Bad Response**

A `400 Bad Request` is returned if:

- The **reset code is invalid** or has expired.
- The **new passwords do not match**.
- The **email provided** does not match any existing accounts.
  
---

### **Email Confirmation**

- After the password is successfully reset, an email is sent to the user's registered email address, confirming the change.
- The email includes details about the successful password reset and any relevant security advice (e.g., if this wasn’t you, please contact support).

---

### **Security Considerations**

- The reset code has a limited lifespan for security purposes.
- Ensure **strong password policies** are enforced (e.g., minimum length, complexity) to protect user accounts.
