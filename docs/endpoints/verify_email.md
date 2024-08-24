# Verify Email

-   **Route:** `/verify/email`
-   **Description:** Verifies the user's email address using the provided verification code. The code is sent to the user's email address upon registration or request.
-   **Method:** `POST`
-   **Request Body:**

    -   **Content-Type:** `application/json`
    -   **Required Fields:**
        ```json
        {
            "email": "user@example.com",
            "code": "123456"
        }
        ```
        -   `email`: The email address of the user that needs to be verified.
        -   `code`: The verification code received by the user via email.

-   **Successful Response:**

    -   **Status Code:** `200 OK`
    -   **Content:**
        ```json
        {
            "msg": "Email verified"
        }
        ```

-   **Bad Response:**

    -   **Status Code:** `400 Bad Request`
    -   **Content:**
        Returned in case of an invalid code or email address.

        ```json
        {
            "msg": ["[Messages.invalid_code]*"],
            "code": ["invalid_code"]
        }
        ```

    -   **Status Code:** `400 Bad Request`
    -   **Content:**
    -   Returned in case of an expired code.
        ```json
        {
            "msg":  ["[Messages.expired_code]*"],
            "code": ["expired_code"]
        }
        ```

    -   **Status Code:** `400 Bad Request`
    -   **Content:**
    -   Returned in case of an unspecified error.
        ```json
        {
            "msg":  ["[Messages.general_msg]*"],
            "code": ["unspecified_error"]
        }
        ```
