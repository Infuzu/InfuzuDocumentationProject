# The IMS LLM Response Endpoint

**Endpoint:**  
`POST https://chat.infuzu.com/api/llm-response`

Use this endpoint to send a series of messages, optionally specify candidate LLMs, and receive a response from the 
chosen LLM. Optionally, you can request that responses be streamed back to you (for supported LLMs).

**Headers:**

- `Content-Type: application/json`
- `Infuzu-API-Key: <your_api_key>` or `X-Infuzu-API-Key: <your_api_key>`

**Request Body Fields:**

```json
{
  "messages": [
    {
      "role": "system",
      "content": "Your initial instruction or context here."
    },
    {
      "role": "user",
      "content": "The user's query or message."
    },
    {
      "role": "assistant",
      "content": "Assistant response (if continuing conversation)."
    }
    // Additional messages as needed...
  ],
  "llms": ["openai.gpt-4o-2024-05-13", "anthropic.claude-3-5-sonnet-20241022"],
  "stream": false
}
```

**Important Requirements:**

1. **Messages Array:**
    - The first message **must** be a `system` or `user` role message.
    - The conversation should alternate logically between `user` and `assistant`.
    - `system` messages can only be the first message.
    - `assistant` messages can only follow `user` messages.
    - Each message must have both `role` and `content` fields.

2. **LLMs Array (Optional):**
    - If provided, IMS will only consider the listed LLMs.
    - If omitted or empty, IMS considers all available LLMs (unless the `exclude_llms` field is specified).
    - If an invalid LLM is specified, an error is returned.

3. **Exclude LLMs Array (Optional):**
    - Use this field to exclude LLMs from consideration.
    - **Mutual Exclusivity:** `exclude_llms` cannot be used in conjunction with `llms`. Only one of these fields can be 
   included in a single request.
    - If `exclude_llms` is provided, IMS with consider all available LLMs except those listed. 
    - If omitted or empty, no LLMs are excluded.

4. **Streaming (Optional):**
    - Set `stream` to `true` to receive responses as a server-sent event (SSE) stream.
    - If `false`, you receive a single JSON response once the answer is ready.
    - If omitted or empty, IMS will default to `false`.

**Mutual Exclusivity of `llms` and `exclude_llms`:
- **Usage Rules:**
  - **Either** provide the `llms` array or the `exclude_llms` array.
  - **Do not** include both `llms` and `exclude_llms` in the same request.
  - **Neither**: If you'd like IMS to consider all available LLMs, include neither `llms` or `exclude_llms`.
- **Potential Errors:**
  - If both `llms` and `exclude_llms` are included in the request, IMS with return a `400 Bad Request` error.

**Currently Supported LLM References:**

- **Anthropic LLMs:**
    - `anthropic.claude-3-5-sonnet-20241022`

- **Google LLMs:**
    - `google.gemini-1.5-pro-002`

- **OpenAI LLMs:**
    - `openai.o1-2024-12-17`
    - `openai.o1-preview-2024-09-12`
    - `openai.o1-mini-2024-09-12`
    - `openai.gpt-4o-2024-11-20`
    - `openai.gpt-4o-2024-08-06`
    - `openai.gpt-4o-2024-05-13`
- **DeepSeek LLMs:**
  - V3: `deepseek.deepseek-chat`

To see the exact pricing and details for these LLMs, refer to the Pricing section of this documentation or visit your 
[Organization’s Pricing Page](https://admin.infuzu.com/o/billing/pricing).

**Example cURL Request (Non-Streaming):**

```bash
curl -X POST https://chat.infuzu.com/api/llm-response \
     -H "Content-Type: application/json" \
     -H "Infuzu-API-Key: $INFUZU_API_KEY" \
     -d '{
       "messages": [
         {"role": "system", "content": "You are a helpful assistant."},
         {"role": "user", "content": "What is the capital of France?"}
       ],
       "llms": ["openai.o1-mini-2024-09-12"],
       "stream": false
     }'
```

**Example cURL Request (Streaming):**

```bash
curl -X POST https://chat.infuzu.com/api/llm-response \
     -H "Content-Type: application/json" \
     -H "Infuzu-API-Key: $INFUZU_API_KEY" \
     -d '{
       "messages": [
         {"role": "system", "content": "You are a helpful assistant."},
         {"role": "user", "content": "Tell me a story."}
       ],
       "llms": ["anthropic.claude-3-5-sonnet-20241022"],
       "stream": true
     }'
```

**Note:** The `llms` field in the examples is optional. If you omit it, IMS will consider all available LLMs and select
the best one based on your request messages.