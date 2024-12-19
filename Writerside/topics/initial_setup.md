# Initial Setup

## Obtaining an API Key
To start using the IMS API, you need a valid **Infuzu API Key**. This key associates your requests with your Infuzu 
account and project, ensuring proper usage tracking and billing.

Steps:

1. **Sign up or Log in**: Visit [Infuzu Admin Dashboard](https://admin.infuzu.com) to create an account or log in if you 
already have one.
2. **Create a Project**: Within the dashboard, [create a new project](https://admin.infuzu.com/o/projects) to manage your 
usage, billing, and settings.
3. **Generate API Key**: Go to your project’s [API Keys page](https://admin.infuzu.com/p/api-keys) and generate an API key. 
Please store it securely; consider using a secure key management system.

## Adding Funds
IMS requires sufficient funds in your Infuzu account to handle billing. You can add funds directly from your account 
dashboard:

1. **Navigate to Billing**: In the Infuzu Dashboard, go to [**Billing Overview**](https://admin.infuzu.com/o/billing/overview) 
page.
2. **Add Funds**: Use your preferred payment method to add credits. Credits correspond directly to the API usage, 
ensuring transparent billing.

## Setting Up Your Environment
You can interact with the IMS API using any HTTP client tool or from your preferred programming language. Here we 
detail how to use curl as a starting point, but you can adapt these steps to other tools.

### Prerequisites
- `curl` installed: Most UNIX-like systems come `curl` pre-installed.
To check:
    ```bash
    curl --version
    ```
  if not installed, follow your OS-specific instructions or visit [curl's official site](https://curl.se/) to install

### Test Your Setup
1. **Set your API key as an environment variable** for convenience:
    <warning>This is not recommended for production environments.</warning>
    ```bash
    export INFUZU_API_KEY="YOUR_INFUZU_API_KEY_HERE"
    ```

2. **Make a test request** to confirm authorization works:
    ```bash
    curl -X POST https://chat.infuzu.com/api/llm-response \
    -H "Content-Type: application/json" \
    -H "Infuzu-API-Key: $INFUZU_API_KEY" \
    -d '{ "messages": [{"role": "system", "content": "Hello, IMS!"}] }'
   ```
    If you receive a response with a `200 OK` and a JSON payload, you’re good to go!

Using Your API Key
You must include your API key in the `Infuzu-API-Key` or `X-Infuzu-API-Key` header for every request. Do not use both 
headers simultaneously. If no valid API key is provided, you’ll receive a `401 Unauthorized` error.