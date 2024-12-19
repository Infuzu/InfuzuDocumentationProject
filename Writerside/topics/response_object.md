# The Response Object

When `stream` is `false`, the IMS API returns a standard JSON response using a common response structure. This 
structure can include `results`, `errors`, and `warnings`. On a successful request, the response will be wrapped in a 
`results` object, and `errors` and `warnings` arrays will be empty.

**Example Non-Streaming Success Response:**

```json
{
  "results": {
    "response": "The capital of France is Paris.",
    "chosen_llm": "openai.gpt-4o-2024-05-13"
  },
  "errors": [],
  "warnings": []
}
```

**Fields in Non-Streaming Success Response:**

- `results`:
    - `response`: A string containing the LLM’s answer or generated text.
    - `chosen_llm`: The identifier (reference) of the LLM chosen by IMS for this request.
- `errors`: An array of error objects (empty on success).
- `warnings`: An array of warning objects (empty on success).

**Error Responses:**

If an error occurs (e.g., invalid request, missing fields, rate limit exceeded), the response will omit `results` and 
instead include `errors`:

```json
{
  "errors": [
    {
      "code": "missing_required_field",
      "message": "The 'messages' field is missing. Please check your request payload."
    }
  ],
  "warnings": []
}
```

**Warnings:**

If there are warnings (e.g., deprecated fields), they appear in the `warnings` array. For most requests, `warnings` 
will be empty.

---

## Streaming Responses (SSE)

When `stream` is `true`, the response is returned as Server-Sent Events (SSE). In this mode, the API does **not** use 
the common `results`, `errors`, and `warnings` wrapper. Instead, it sends raw JSON objects as SSE messages.

**Example SSE Sequence:**

- **Initial SSE Message (Chosen LLM)**:

  ```json
  {"chosen_llm": "openai.gpt-4o-2024-05-13"}
  ```

  This indicates which LLM IMS selected for this request.

- **Subsequent SSE Messages (Response Chunks)**:

  ```json
  {"response": "Once upon a time, in a land far away,"}
  {"response": "there lived a wise old owl who ..."}
  {"response": "... and they lived happily ever after."}
  ```

Each SSE message is a small JSON object containing either a `chosen_llm` (for the first message) or a `response` field 
representing a chunk of the LLM-generated text. The stream ends once the entire response has been sent. No `errors`, 
`results`, or `warnings` fields are present in SSE messages.

If an error occurs during streaming, the connection will be terminated, and no further messages will be sent. In such 
cases, the error would have been handled before streaming began or at the start of the SSE stream if an early error 
occurred.

---

**Summary:**

- **Non-Streaming Mode (`stream: false`)**:  
  Returns a JSON object with `results`, `errors`, and `warnings`. On success, `results` includes `response` and 
`chosen_llm`.

- **Streaming Mode (`stream: true`)**:  
  Returns a series of SSE JSON messages. The first SSE message includes `chosen_llm`, and subsequent messages include 
`response` chunks. No `results`, `errors`, or `warnings` arrays are present in this mode.

By understanding this structure, you can confidently parse the IMS API responses in both non-streaming and streaming 
scenarios.