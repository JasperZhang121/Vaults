### DeepSeek (Chat Completions) — Stream (SSE)

```bash
curl https://api.deepseek.example/v1/chat/completions \
  -H "Authorization: Bearer $DEEPSEEK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "deepseek-chat",
    "messages": [
      {"role":"user","content":"Say hello."}
    ],
    "stream": true
  }'
```

```text
data: {"id":"ds-xyz","object":"chat.completion.chunk","created":1710001000,"model":"deepseek-chat","choices":[{"index":0,"delta":{"role":"assistant","content":"Hel"},"finish_reason":null}]}
data: {"id":"ds-xyz","object":"chat.completion.chunk","created":1710001001,"model":"deepseek-chat","choices":[{"index":0,"delta":{"content":"lo"},"finish_reason":null}]}
data: {"id":"ds-xyz","object":"chat.completion.chunk","created":1710001002,"model":"deepseek-chat","choices":[{"index":0,"delta":{"content":"!"},"finish_reason":null}]}
data: {"id":"ds-xyz","object":"chat.completion.chunk","created":1710001003,"model":"deepseek-chat","choices":[{"index":0,"delta":{},"finish_reason":"stop"}]}
data: [DONE]
```

### DeepSeek (Reasoning Model) — Stream with Reasoning

```text
data: {"id":"ds-abc","object":"chat.completion.chunk","model":"deepseek-reasoner",
       "choices":[{"index":0,"delta":{"role":"assistant","reasoning_content":"First, break the problem into smaller steps…"}}]}

data: {"id":"ds-abc","object":"chat.completion.chunk","model":"deepseek-reasoner",
       "choices":[{"index":0,"delta":{"reasoning_content":"From these steps, we conclude…"}}]}

data: {"id":"ds-abc","object":"chat.completion.chunk","model":"deepseek-reasoner",
       "choices":[{"index":0,"delta":{"content":"Final answer: A."}}]}

data: {"id":"ds-abc","object":"chat.completion.chunk","model":"deepseek-reasoner",
       "choices":[{"index":0,"delta":{},"finish_reason":"stop"}]}
data: [DONE]
```
