### OpenAI (Chat Completions) — Non-Stream

```bash
curl https://api.openai.com/v1/chat/completions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o-mini",i
    "messages": [
      {"role":"system","content":"You are a helpful assistant."},
      {"role":"user","content":"Say hello."}
    ],
    "temperature": 0.7
  }'
```

```json
{
  "id": "chatcmpl-abc123",
  "object": "chat.completion",
  "created": 1710000000,
  "model": "gpt-4o-mini",
  "choices": [
    {
      "index": 0,
      "message": {"role": "assistant", "content": "Hello!"},
      "finish_reason": "stop"
    }
  ],
  "usage": {"prompt_tokens": 13, "completion_tokens": 2, "total_tokens": 15}
}
```

### OpenAI (Chat Completions) — Stream (SSE)

```bash
curl https://api.openai.com/v1/chat/completions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o-mini",
    "messages": [
      {"role":"user","content":"Say hello."}
    ],
    "stream": true
  }'
```

```text
data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","created":1710000000,"model":"gpt-4o-mini","choices":[{"index":0,"delta":{"role":"assistant","content":"Hel"},"finish_reason":null}]}
data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","created":1710000001,"model":"gpt-4o-mini","choices":[{"index":0,"delta":{"content":"lo"},"finish_reason":null}]}
data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","created":1710000002,"model":"gpt-4o-mini","choices":[{"index":0,"delta":{"content":"!"},"finish_reason":null}]}
data: {"id":"chatcmpl-abc123","object":"chat.completion.chunk","created":1710000003,"model":"gpt-4o-mini","choices":[{"index":0,"delta":{},"finish_reason":"stop"}]}
data: [DONE]
```

### OpenAI (Responses API) — Stream with Reasoning Events

```text
event: response.created
data: {"id":"resp_123","model":"o3","type":"response.created"}

event: response.output_text.delta
data: {"id":"resp_123","delta":"Conclusion first: ","output_index":0}

event: response.reasoning_summary_text.delta
data: {"id":"resp_123","delta":"(Reasoning) The problem has two parts: first … then …","summary_index":0}

event: response.output_text.delta
data: {"id":"resp_123","delta":"Therefore, the answer is A.","output_index":0}

event: response.completed
data: {"id":"resp_123",
       "usage":{"input_tokens":123,
                "output_tokens":456,
                "output_tokens_details":{"reasoning_tokens":120}}}
```
