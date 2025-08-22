### Nacos

- **Definition**: An open-source **service registry and configuration center** (from Alibaba).
    
- **Purpose**: Acts like a **phone book** for microservices, helping them register and discover each other.
    

#### Example

- Services:
    
    - `UserService → 10.0.0.1:8080`
        
    - `OrderService → 10.0.0.2:8080`
        
- Instead of hardcoding IPs, they register in **Nacos**.
    
- A client asks: _“Where is OrderService?”_ → Nacos provides the address.

### MCP (Model Context Protocol)

- **Definition**: A protocol standard for **AI agents** to connect to external tools (APIs, DBs, services).
    
- **Purpose**: Unified way for LLMs (e.g., ChatGPT, Claude) to call tools.

#### Example

- Task: Get weather info.
    
- Without MCP: Custom plugin needed.
    
- With MCP: Any weather API exposes itself as an **MCP server**.
    
- Agent says: _“What’s the weather in Beijing?”_ → MCP server responds in a standard format.

### Nacos MCP Registry

- **Definition**: Extension of Nacos to manage **MCP tools**.
    
- **Purpose**: Automatically adapts existing APIs into MCP tools, with **zero code changes**.
    
- **Role**: Works as a **control plane** for registering and managing tools.

#### Example

- REST API:
    
    ```
    GET /api/weather?city=Beijing
    ```
    
- Registry adapts it:
    
    - Tool: `WeatherService`
        
    - Input: `city`
        
    - Output: `temperature, humidity`
        
- AI agents can now discover and invoke it like a native MCP tool.

### Nacos MCP Router

- **Definition**: A special **MCP server** that works as a hub or gateway.
    
- **Purpose**: Centralizes agent–tool interaction into **one endpoint**.
    
- **Modes**:
    
    1. **Proxy Mode** – Wraps legacy MCP servers to modern protocols.
        
    2. **Router Mode** – Uses semantic search to pick the right tool for a task.
        

#### Example

- Registered tools:
    
    - `WeatherService`
        
    - `StockService`
        
- User asks: _“Stock price of Alibaba?”_
    
    - Router → `StockService`.
        
- User asks: _“Weather in Beijing?”_
    
    - Router → `WeatherService`.
        
- Agent only knows **the Router**, not each service.

### MCP Zero

- **Definition**: A new integration pattern using **only one MCP service (the Router)**.
    
- **Purpose**: Simplifies tool management → the router dynamically exposes _all tools_.
    

#### Example

```python
from langchain_mcp_adapters.client import MultiServerMCPClient

client = MultiServerMCPClient({
    "mcp_router": {
        "url": "http://localhost:8000/sse/",
        "transport": "sse",
    }
})

tools = await client.get_tools()
# Returns WeatherService, StockService, TranslateService...
```

- One registration → access to many tools.

### Summary Table

| Concept                | What It Does                                     | Example                                   |
| ---------------------- | ------------------------------------------------ | ----------------------------------------- |
| **Nacos**              | Registry for microservices                       | Find `OrderService` by name               |
| **MCP**                | Protocol for LLM ↔ tool communication            | Ask weather via MCP                       |
| **Nacos MCP Registry** | Adapts APIs into MCP tools                       | Turn `/api/weather` into `WeatherService` |
| **Nacos MCP Router**   | Single entry point; routes requests to tools     | Routes “stock price” → `StockService`     |
| **MCP Zero**           | Only register the Router; all tools auto-exposed | One config, many tools                    |
