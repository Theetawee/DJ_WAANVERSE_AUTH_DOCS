# Dj Waanverse Auth Client Hints Middleware Documentation

## Overview

The Client Hints Middleware is designed to process and store browser client hints. It provides detailed client information that can be accessed within Django views, including details about the browser, device, network conditions, and user preferences. This allows for personalized and adaptive content based on the client's capabilities.

## Installation

To integrate the middleware into your Django project, add the following to your `MIDDLEWARE` setting in `settings.py`. Ensure that it is placed after the security middlewares but before any content processing middlewares:

```python
MIDDLEWARE = [
    'dj_waanverse_auth.middleware.client_hints.ClientHintsMiddleware',
]
```

## Data Categories

### Browser Information

These client hints provide details about the browser and its capabilities, helping you adapt content based on the user's browser type and version:

-   **Sec-CH-UA**: This field contains the brand and version of the browser the client is using (e.g., "Chrome", "Firefox").
-   **Sec-CH-UA-Full-Version**: Provides the full version number of the browser, including major, minor, and patch versions (e.g., "91.0.4472.124").
-   **Sec-CH-UA-Full-Version-List**: A list of browsers and their full versions that the client might use, providing a broader context of the client’s browser ecosystem.
-   **Sec-CH-UA-WOW64**: A boolean indicating whether the client is running a 64-bit version of Windows on a 32-bit system (Windows-on-Windows 64-bit, or WOW64).
-   **Sec-CH-UA-Form-Factor**: Represents the form factor of the client device, such as "mobile", "desktop", or "tablet". This helps in tailoring content based on whether the client is using a desktop or mobile device.

### Device Information

These hints provide details about the hardware and platform the client is using:

-   **Sec-CH-UA-Platform**: The operating system of the device (e.g., "Windows", "macOS", "Linux").
-   **Sec-CH-UA-Platform-Version**: The version of the operating system the client is running (e.g., "10.0.18363").
-   **Sec-CH-UA-Arch**: The CPU architecture of the client device (e.g., "x86", "arm64").
-   **Sec-CH-UA-Model**: The model of the device (e.g., "iPhone", "Pixel 4").
-   **Sec-CH-UA-Mobile**: A boolean indicating whether the device is a mobile device (e.g., `True` for mobile, `False` for desktop).
-   **Device-Memory**: The amount of memory (RAM) on the client device, in gigabytes (e.g., `4.0` for 4 GB).
-   **Sec-CH-Device-Memory**: The device memory as a string, often used for more granular representation.
-   **DPR (Device Pixel Ratio)**: A numeric value representing the ratio between the physical pixels and the device's logical pixels. A higher DPR usually indicates a high-resolution screen.
-   **Sec-CH-DPR**: The device pixel ratio, represented as a string.
-   **Sec-CH-Width**: The width of the viewport (browser window) in pixels.
-   **Sec-CH-Viewport-Width**: The width of the viewport, which may vary depending on the device's screen size and resolution.
-   **Sec-CH-Viewport-Height**: The height of the viewport, used for responsive design.
-   **Sec-CH-Device-Type**: The type of device, such as "mobile", "desktop", or "tablet".
-   **Sec-CH-UA-Platform-Arch**: The architecture of the platform (e.g., "x64", "arm").
-   **Sec-CH-Bitness**: Indicates the bitness of the platform (e.g., "32-bit" or "64-bit").

### Network Information

These client hints provide details about the client’s network connection, enabling adaptive content delivery based on network speed and connectivity:

-   **Downlink**: The download speed of the client's connection, measured in megabits per second (Mbps).
-   **ECT (Effective Connection Type)**: The effective connection type, such as "slow-2g", "2g", "3g", "4g", indicating the quality of the network connection.
-   **RTT (Round-Trip Time)**: The round-trip time in milliseconds, indicating the latency of the network connection.
-   **Save-Data**: A boolean that indicates if the user has enabled a data-saving mode in their browser.
-   **Sec-CH-Downlink**: The downlink speed represented as a string (e.g., "1.0" for 1 Mbps).
-   **Sec-CH-Downlink-Max**: The maximum downlink speed that the client can achieve, useful for understanding network performance.
-   **Sec-CH-Connection-Type**: The connection type (e.g., "wifi", "cellular", "ethernet").

### User Preferences

These hints describe user preferences, which can be used to provide a more personalized experience:

-   **Sec-CH-Prefers-Color-Scheme**: The user's preferred color scheme, typically "light" or "dark", which is used for adapting UI themes.
-   **Sec-CH-Prefers-Reduced-Motion**: A boolean indicating whether the user prefers reduced motion or animations in the UI (often due to accessibility needs).
-   **Sec-CH-Prefers-Contrast**: The user’s preferred contrast setting, used to optimize UI readability based on user preferences.
-   **Sec-CH-Prefers-Reduced-Data**: A boolean indicating whether the user prefers reduced data usage, often used to adjust content quality.
-   **Sec-CH-Forced-Colors**: A boolean indicating if forced color schemes are applied (typically in cases of high contrast or accessibility modes).

## Usage Examples

### Basic Usage in Views

```python
from django.http import JsonResponse

def basic_view(request):
    platform = request.client_info.device.get('Sec_Ch_Ua_Platform', 'unknown')
    is_mobile = request.client_info.device.get('Sec_Ch_Ua_Mobile')
    color_scheme = request.client_info.preferences.get('Sec_Ch_Prefers_Color_Scheme')

    return JsonResponse({
        'platform': platform,
        'is_mobile': is_mobile,
        'color_scheme': color_scheme
    })
```

### Advanced Usage Examples

#### Responsive Design Decisions

```python
def responsive_view(request):
    device_info = request.client_info.device
    dpr = device_info.get('Dpr', 1.0)
    viewport_width = device_info.get('Sec_Ch_Viewport_Width', '1024')
    is_mobile = device_info.get('Sec_Ch_Ua_Mobile', False)

    template = 'mobile.html' if is_mobile else 'desktop.html'
    context = {
        'high_res': dpr > 1.5,
        'viewport_width': viewport_width
    }

    return render(request, template, context)
```

#### Dark Mode Detection

```python
def theme_aware_view(request):
    preferences = request.client_info.preferences
    color_scheme = preferences.get('Sec_Ch_Prefers_Color_Scheme', 'light')
    reduced_motion = preferences.get('Sec_Ch_Prefers_Reduced_Motion', False)

    context = {
        'dark_mode': color_scheme == 'dark',
        'disable_animations': reduced_motion
    }

    return render(request, 'theme_aware.html', context)
```

#### Network-Aware Content

```python
def adaptive_content_view(request):
    network = request.client_info.network
    connection_type = network.get('Sec_Ch_Connection_Type', 'unknown')
    save_data = network.get('Save_Data', False)
    downlink = network.get('Downlink', -1)

    if save_data or downlink < 1.0:
        return render(request, 'low_bandwidth.html')

    return render(request, 'high_bandwidth.html')
```

## Best Practices

1. **Defaults**: Always provide default values when accessing client hints.
2. **Boolean Handling**: Ensure that boolean values are checked properly (e.g., `None` for undefined states).
3. **Serializers**: Use serializers when working with API responses to ensure clean, structured data.

## Error Handling

The middleware provides safe defaults for missing or undefined values:

-   String fields default to "Unknown"
-   Boolean fields return `None`
-   Numeric fields return `-1`

## Performance Considerations

-   The middleware processes the headers once per request.
-   Processed data is stored in `request.client_info` for efficient subsequent access.
-   The performance overhead of processing headers is minimal.

## Security Considerations

-   Client hints are self-reported and can be spoofed. Do not rely on them for security-related decisions.
-   Use client hints primarily for optimization and customization of the user experience.

## Debugging

To debug the client hints being sent by the client, use the following:

```python
def debug_view(request):
    json_data = ClientHintsMiddleware.get_client_info_json(request)
    return JsonResponse({'client_info': json.loads(json_data)})
```

## Common Issues and Solutions

### Missing Values

If a value is missing or unknown, use fallback logic or default values.

```python
platform = request.client_info.device.get('Sec_Ch_Ua_Platform')
if platform == "Unknown":
    # Use fallback detection
```

### Boolean Interpretation

If a boolean value is `None`, handle it appropriately.

```python
is_mobile = request.client_info.device.get('Sec_Ch_Ua_Mobile')
if is_mobile is None:
    # Use fallback or assumptions
```

### Numeric Conversion

If a numeric value is `-1`, it indicates missing data. Use a default value in such cases.

```python
memory = request.client_info.device.get('Device_Memory', -1)
if memory == -1:
    # Provide default memory value
```
