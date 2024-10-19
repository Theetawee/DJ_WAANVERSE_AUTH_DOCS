## **Reset Password Endpoint**

- **Route:** `POST /password/reset`
- **Description:** Initiates the process for resetting a user's password by sending a reset code to the email address provided.

---

### **Request Details**

- **Method:** `POST`
- **Content-Type:** `application/json`

### **Request Body:**

```json
{
    "email": "string"
}
```

- **Required Field:**
    - **email**: The registered email address of the user requesting the password reset.

---

### **Successful Response**

- When the password reset code is successfully sent to the user’s email.
- **Status Code:** `200 OK`
- **Response Content:**

```json
{
    "msg": "[Messages.password_reset_code_sent]*",
    "email": "string"
}
```

- **msg**: Confirmation message indicating that the reset code has been sent.
- **email**: The email address to which the reset code was sent.

---

### **Bad Response**

A `400 Bad Request` is returned if:

- **No account** is associated with the provided email.
- **Invalid email** format is provided.
- **Cooldown period** hasn’t expired after a previous reset request.
