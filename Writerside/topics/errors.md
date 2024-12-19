# Errors

Infuzu’s IMS API returns standard HTTP status codes and provides a structured JSON error response.

**Common Error Fields:**

- `code`: A machine-readable error code.
- `message`: A human-readable explanation.

**Error Structure Example:**

```json
{
  "error": {
    "code": "missing_api_key",
    "message": "The 'Infuzu-API-Key' or 'X-Infuzu-API-Key' header is missing."
  }
}
```

**Common Error Codes and Meanings:**

| HTTP Status                | Code                   | Meaning                                                                           |
|----------------------------|------------------------|-----------------------------------------------------------------------------------|
| 400 Bad Request            | invalid_request_body   | Your request body is malformed or missing required fields.                        |
| 400 Bad Request            | multiple_api_keys      | Both `Infuzu-API-Key` and `X-Infuzu-API-Key` were provided, which is not allowed. |
| 400 Bad Request            | missing_required_field | A required field (e.g., `messages`) is missing or empty.                          |
| 400 Bad Request            | invalid_field          | A field’s value is not allowed (e.g., invalid `role`, invalid `LLM`).             |
| 401 Unauthorized           | missing_api_key        | No valid API key was provided.                                                    |
| 401 Unauthorized           | invalid_api_key        | The provided API key is invalid.                                                  |
| 402 Payment Required       | not_enough_funds       | Your account does not have enough funds to process the request.                   |
| 415 Unsupported Media Type | unsupported_media_type | The `Content-Type` header is not `application/json`.                              |
| 429 Too Many Requests      | rate_limit_exceeded    | You have exceeded your rate limit for requests or input characters.               |
| 500 Internal Server Error  | internal_server_error  | A server-side error occurred. Contact Infuzu support if this persists.            |

**Handling Errors:**

- **Check HTTP Status:** Start by examining the HTTP status code.
- **Examine `code` and `message`:** The `code` provides a stable way to handle specific error scenarios. The `message` 
gives more context.
- **Take Corrective Action:**
    - For `missing_api_key`: Provide a valid API key.
    - For `not_enough_funds`: Add funds to your account.
    - For `rate_limit_exceeded`: Wait for your daily limit to reset or upgrade your plan.
    - For `invalid_field`: Correct your request payload to use valid roles, messages, or LLMs.

By following these guidelines and referencing the error codes above, you can quickly diagnose and fix issues in your 
integration.
