# Dj Waanverse Auth — Middleware Guide

This document explains the middlewares provided by the `dj-waanverse-auth` package, what they do, and how to apply them in your Django project.

---

## 📦 Available Middlewares

### 1. `AuthCookieMiddleware` (Required)

**Purpose:**
Removes unnecessary cookies from the response to keep the client's session clean and secure.

---

### 2. `ClientHintsMiddleware`

**Purpose:**
Extracts detailed client information from HTTP headers (like device type, browser, and user preferences) and makes them accessible in your views via `request.client_info`.

**Use case:**
Useful for customizing responses based on client characteristics (e.g., rendering different content for mobile vs. desktop).

**Example:**

```python
def my_view(request):
    hints = request.client_info
    print(hints.get('browser'))  # Access user-agent string
```

Example of Hints structure:

```python
{
  "browser": {
    "Sec_Ch_Ua": "unknown",
    "Sec_Ch_Ua_Full_Version": "unknown",
    "Sec_Ch_Ua_Full_Version_List": "unknown",
    "Sec_Ch_Ua_Wow64": None,
    "Sec_Ch_Ua_Form_Factor": "unknown"
  },
  "device": {
    "Sec_Ch_Ua_Platform": "unknown",
    "Sec_Ch_Ua_Platform_Version": "unknown",
    "Sec_Ch_Ua_Arch": "unknown",
    "Sec_Ch_Ua_Model": "unknown",
    "Sec_Ch_Ua_Mobile": None,
    "Device_Memory": -1,
    "Sec_Ch_Device_Memory": "unknown",
    "Dpr": -1,
    "Sec_Ch_Dpr": "unknown",
    "Sec_Ch_Width": "unknown",
    "Sec_Ch_Viewport_Width": "unknown",
    "Sec_Ch_Viewport_Height": "unknown",
    "Sec_Ch_Device_Type": "unknown",
    "Sec_Ch_Ua_Platform_Arch": "unknown",
    "Sec_Ch_Bitness": "unknown"
  },
  "network": {
    "Downlink": -1,
    "Ect": "unknown",
    "Rtt": -1,
    "Save_Data": None,
    "Sec_Ch_Downlink": "unknown",
    "Sec_Ch_Downlink_Max": "unknown",
    "Sec_Ch_Connection_Type": "unknown"
  },
  "preferences": {
    "Sec_Ch_Prefers_Color_Scheme": "unknown",
    "Sec_Ch_Prefers_Reduced_Motion": None,
    "Sec_Ch_Prefers_Contrast": "unknown",
    "Sec_Ch_Prefers_Reduced_Data": None,
    "Sec_Ch_Forced_Colors": None
  }
}


```

---

### 3. `IPBlockerMiddleware`

**Purpose:**
Blocks access from specific IP addresses or ranges defined in your Django settings.

**Use case:**
To prevent abuse or restrict access to parts of your system by IP address.

**Required setting in `settings.py`:**

```python
BLOCKED_IPS = [
    "192.168.0.0/24",
    "203.0.113.45",
]

ALLOWED_IPS=[
    "127.0.0.1"
]
```

**If an IP is in the `BLOCKED_IPS` list, the middleware blocks the request. If it's in the `ALLOWED_IPS` list, the middleware allows the request even if it's in the `BLOCKED_IPS` list.**

## ⚙️ How to Apply the Middlewares

To enable any of the middlewares, add them to your `MIDDLEWARE` list in `settings.py` **in the correct order**, depending on what you want to happen first.

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',

    # Dj Waanverse Auth Middlewares
    'dj_waanverse_auth.middleware.auth.AuthCookieMiddleware',
    'dj_waanverse_auth.middleware.client_hints.ClientHintsMiddleware',
    'dj_waanverse_auth.middleware.auth.IPBlockerMiddleware',

    'django.middleware.common.CommonMiddleware',
    ...
]
```

---

## 🧪 Middleware Access Notes

-   `ClientHintsMiddleware` attaches a `client_hints` attribute to the request object.
-   `IPBlockerMiddleware` blocks requests at the middleware level — the view won't even be reached.
-   `AuthCookieMiddleware` modifies the response object after the view is called.

---

## ✅ Summary

| Middleware              | Description                                       | Config Required       |
| ----------------------- | ------------------------------------------------- | --------------------- |
| `AuthCookieMiddleware`  | Cleans up unnecessary cookies                     | No                    |
| `ClientHintsMiddleware` | Adds browser/device info to `request.client_info` | No                    |
| `IPBlockerMiddleware`   | Blocks requests from listed IPs                   | `BLOCKED_IPS` setting |
