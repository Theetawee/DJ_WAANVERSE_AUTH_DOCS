# Auth Cookie Middleware Documentation

This middleware automatically clears unnecessary cookies from the response to keep the client's session clean and secure. It handles cookies in a way that ensures optimal security and performance without requiring manual intervention.

---

### **Installation**

The **Auth Cookie Middleware** is automatically integrated into your project and does not require any manual setup from the user. It will work out-of-the-box as long as the middleware package is correctly installed.

Once the package is installed and configured in the `MIDDLEWARE` setting, it will automatically clear unnecessary cookies from the response.

```python
MIDDLEWARE = [
    # ... other middleware
    'dj_waanverse_auth.middleware.auth.AuthCookieMiddleware',
]
```

### **How It Works**

The middleware intercepts the response before it is sent back to the client. It looks for any unnecessary or outdated authentication-related cookies and removes them. This helps in keeping the session data clean and secure.
