# hcap.ai API Documentation

Welcome to the hcap.ai API documentation. This guide covers the available endpoints for interacting with our AI models, including listing models, generating chat completions, and checking usage quotas.

---

## Base URL
All endpoints are accessible via your base API URL, e.g., `https://api.hcap.ai`.

---

## Authentication
All requests require authorization via the `Authorization` header with a valid API key:

```
Authorization: Bearer YOUR_API_KEY
```

---

## Endpoints

### 1. List Available Models
**Endpoint:** `/v1/models`

**Description:**  
Retrieve a list of available models for both image and text generation.

**Request:**

```http
GET /v1/models
```

**Response Example:**

```json
{
  "object": "list",
  "data": [
    {
      "id": "gpt-4o",
      "created": 0,
      "owned_by": "openai",
      "endpoint_url": "/v1/chat/completions",
      "object": "model",
      "multiplier": 1
    }
    // ... additional models
  ]
}
```

**Response Fields:**

| Field          | Description                                                                  |
|----------------|------------------------------------------------------------------------------|
| `id`           | Unique identifier for the model (used in requests)                         |
| `created`      | Timestamp of model creation (Unix epoch)                                     |
| `owned_by`     | Provider or owner of the model                                               |
| `endpoint_url` | The API endpoint to use for generating completions with this model         |
| `object`       | Type of object, typically `"model"`                                         |
| `multiplier`   | Token multiplier affecting billing (input tokens x multiplier + output tokens x multiplier) |

---

### 2. Chat Completion
**Endpoint:** `/v1/chat/completions`

**Description:**  
Generate a conversational response based on a sequence of messages.

**Request:**

```json
POST /v1/chat/completions
Content-Type: application/json
Authorization: Bearer YOUR_API_KEY

{
  "model": "model_id",
  "messages": [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Hello, who won the world series in 2020?"}
  ],
  "stream": false
}
```

**Parameters:**

| Parameter   | Type    | Description                                         | Notes                                              |
|-------------|---------|-----------------------------------------------------|----------------------------------------------------|
| `model`   | string  | ID of the model to use                              | Retrieved from `/v1/models` endpoint              |
| `messages` | array of objects | List of message objects representing the conversation | Each object should have `role` and `content` fields |
| `stream`   | boolean | Determines if response should be streamed          | `true` for streaming output, `false` for complete response |

**Note:**  
Currently, parameters such as `max_tokens` and `temperature` are not supported but are planned for future implementation.

**Response:**

- If `stream` is `false`, a complete response containing the generated reply will be returned.
- If `stream` is `true`, a streaming response will be provided (not shown here).

---

### 3. Usage and Quota Check
**Endpoint:** `/v1/api-keys/usage`

**Description:**  
Retrieve remaining usage quotas for your API key.

**Request:**

```http
GET /v1/api-keys/usage
Authorization: Bearer YOUR_API_KEY
```

**Response Example:**

```json
{
  "tier": "premium",
  "token_usage_today": 5000,
  "image_usage_today": 250,
  "daily_token_limit": 100000,
  "daily_image_limit": 1000,
  "remaining_token_quota": 95000,
  "remaining_image_quota": 750,
  "created_at": "2024-01-15T10:00:00Z",
  "discord_id": "1234567890"
}
```

**Response Fields:**

| Field                     | Description                                                     |
|---------------------------|-----------------------------------------------------------------|
| `tier`                   | Subscription tier or plan                                       |
| `token_usage_today`      | Number of tokens used today                                      |
| `image_usage_today`      | Number of images generated today                                  |
| `daily_token_limit`      | Daily token limit (could be `"unlimited"` if no limit)        |
| `daily_image_limit`      | Daily image limit (could be `"unlimited"`)                     |
| `remaining_token_quota`  | Remaining token quota for the day                                |
| `remaining_image_quota`  | Remaining image generation quota for the day                    |
| `created_at`             | Account creation timestamp                                       |
| `discord_id`             | Associated Discord user ID (if applicable)                      |

---

## Additional Notes
- All endpoints require proper API key authorization.
- Future updates will include support for `max_tokens`, `temperature`, and other parameters.
- Utilize `/v1/models` to discover available models and their capabilities.

---

For further assistance or inquiries, please contact our support team or refer to our official documentation on [hcap.ai](https://hcap.ai).
