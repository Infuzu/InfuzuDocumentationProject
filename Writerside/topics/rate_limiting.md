# Rate Limiting

Infuzu applies **dynamic rate limits** to ensure fair usage and maintain system stability. Rate limits define how many 
requests or how much content you can process within given rolling time windows. While there are default starting limits, 
Infuzu continuously monitors your usage patterns and may adjust your rate limits over time.

**Important:**  
Your specific rate limits may differ from the defaults based on your usage. Always check your 
[current rate limits](https://admin.infuzu.com/o/limits/rate-limits) in 
the [Infuzu Admin Dashboard](https://admin.infuzu.com/) to stay informed about your actual quotas.

## Default Rate Limits

By default, Infuzu sets the following rate limits for requests and input characters. These limits apply to the IMS LLM 
Response endpoint and are measured over rolling time windows.

| Metric                                          | Description                                                                             | Default Limit       | Time Window |
|-------------------------------------------------|-----------------------------------------------------------------------------------------|---------------------|-------------|
| **LLM Response Requests in a Minute**           | The maximum number of requests you can make to the LLM Response endpoint in one minute. | 100 requests/min    | 1 minute    |
| **LLM Response Requests in 10 Minutes**         | The maximum number of requests you can make in ten minutes.                             | 500 requests/10 min | 10 minutes  |
| **LLM Response Requests in an Hour**            | The maximum number of requests you can make in one hour.                                | 1500 requests/hour  | 1 hour      |
| **LLM Response Input Characters in a Minute**   | The maximum number of input characters you can send in one minute.                      | 10,000 chars/min    | 1 minute    |
| **LLM Response Input Characters in 10 Minutes** | The maximum number of input characters you can send in ten minutes.                     | 50,000 chars/10 min | 10 minutes  |
| **LLM Response Input Characters in an Hour**    | The maximum number of input characters you can send in one hour.                        | 150,000 chars/hour  | 1 hour      |

If you exceed these limits, the API returns a `429 Too Many Requests` error, indicating that you must wait until the 
limit resets or upgrade your plan.
<warning>
Even when you receive a 429 Too Many Requests error due to exceeding rate limits, the attempted request still counts 
toward your usage quotas. Implementing a backoff strategy is essential to handle these errors gracefully and avoid 
unnecessary charges.
</warning>

## Dynamic Adjustments

Infuzu’s rate limits are not static. Over time, Infuzu periodically analyzes your usage patterns and may increase your 
rate limits if it detects sustained higher usage. This process ensures that your application can grow without 
constantly hitting default limits.

**How It Works:**

1. **Usage Monitoring**: Infuzu tracks your request and input character usage over time.
2. **Periodic Evaluation**: A background process periodically reviews your historical usage 
(e.g., looking back over the past weeks or months).
3. **Dynamic Limit Setting**: If your usage regularly approaches or exceeds default limits, Infuzu may raise your 
limits to accommodate your growth—without you having to request it.
4. **Dashboard Visibility**: Updated limits are reflected in your organization’s settings in the 
[Infuzu Admin Dashboard](https://admin.infuzu.com/).

## Checking Your Current Rate Limits

Because rate limits can evolve with your usage, always refer to your organization’s settings in the dashboard for the 
most accurate, up-to-date information. If you find that the default limits or your current adjusted limits are 
insufficient, contact Infuzu support to discuss custom rate limit solutions.

## Customization

If your organization needs different or higher quotas than what the dynamic system provides, Infuzu’s support team can 
work with you to implement custom rate limits tailored to your project’s needs.
