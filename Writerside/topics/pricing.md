# Pricing

The Infuzu IMS API uses a **pay-as-you-go** model. Each request consumes credits based on the chosen LLM and the 
complexity of the prompt and response. The prices below are Infuzu’s **default** pricing. Your organization may have 
custom or discounted rates. To view your specific rates, please check your organization’s 
[pricing page](https://admin.infuzu.com/o/billing/pricing) in the [Infuzu Admin Dashboard](https://admin.infuzu.com/).

## How IMS Pricing Works

- **Request Processing**: Each request is evaluated against one or more LLMs to find the optimal one for your input.
  
- **Multiple LLMs Consideration**: If you supply multiple LLMs in the `llms` field or omit this field to consider all 
available LLMs, IMS determines costs as follows:
  
  - The input character cost is based on the **most expensive** input character rate among the considered LLMs.
  - The output character cost is based on the **most expensive** output character rate among the considered LLMs.
  
  <tip>
  This ensures you have the freedom to explore multiple LLMs without losing predictability. If you already know your 
  preferred LLM, you can specify only one to lock in its pricing.
  </tip>
  
  ```mermaid
  graph TD
    A[Send Request] --> B[Evaluate LLMs]
    B --> C{Determine Most Expensive Rates}
    C --> D[Calculate Input Cost]
    C --> E[Calculate Output Cost]
    D --> F[Total Cost]
    E --> F
    F --> G[Deduct Credits]
  ```

- **Input & Output Size**: The cost scales with the number of input characters and output characters processed.

## Default Pricing Chart

| LLM                                        | Price per 1M Input Chars | Price per 1M Output Chars |
|--------------------------------------------|--------------------------|---------------------------|
| OpenAI - o1-preview (2024-09-12)           | `$4.3125`                | `$17.25`                  |
| OpenAI - o1-mini (2024-09-12)              | `$0.8625`                | `$3.45`                   |
| OpenAI - GPT-4o (2024-05-13)               | `$1.4375`                | `$4.3125`                 |
| Google - Gemini 1.5 Pro 002                | `$1.4375`                | `$2.875`                  |
| Anthropic - Claude 3.5 Sonnet (2024-10-22) | `$0.8625`                | `$4.3125`                 |

<warning>
**Note**: These prices are subject to change. Always refer to your Organization’s Pricing Page for the most up-to-date 
information.
</warning>

### Example Calculation

Suppose you send a request with the following parameters:

- **Selected LLMs**: `["gpt-3.5-turbo", "gpt-4"]`
- **Input Characters**: 500,000
- **Output Characters**: 1,000,000

**Calculation Steps**:

1. **Determine the Most Expensive Rates**:
    - **Input**: Assume `gpt-4` has the highest input rate at `$1.4375` per 1M input chars.
    - **Output**: Assume `gpt-4` also has the highest output rate at `$4.3125` per 1M output chars.

2. **Calculate Costs**:
    - **Input Cost**: `(500,000 / 1,000,000) * $1.4375 = $0.71875`
    - **Output Cost**: `(1,000,000 / 1,000,000) * $4.3125 = $4.3125`

3. **Total Cost**: `$0.71875 + $4.3125 = $5.03125`

This ensures predictable billing even when multiple LLMs are considered.

## Billing Process

1. **Request & Response**: You send a request to the IMS endpoint. IMS considers the provided `llms` list or all 
available LLMs if none are specified.
2. **Usage Calculation**: IMS determines the costs by considering the highest input and output rates among the 
candidate LLMs and applies it to the number of input/output characters.
3. **Credit Deduction**: Funds are deducted from your Infuzu account balance accordingly.

If your account does not have enough funds, the request will fail with a `402 Payment Required` status code, prompting 
you to add more funds.

<warning>
**Next Steps**: Make sure you have sufficient credits in your account. If your usage scales or your organization's 
rates differ, review your billing and top up funds as needed.
</warning>
```