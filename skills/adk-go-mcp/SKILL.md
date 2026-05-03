---
name: adk-go-mcp
description: Expert guidance for implementing Model Context Protocol (MCP) toolsets in ADK-Go.
---

# ADK-Go MCP Toolset

Use this skill to integrate external tools via the Model Context Protocol (MCP).

## 🛠️ Mandatory Imports

```go
import (
    "github.com/modelcontextprotocol/go-sdk/mcp"
    "google.golang.org/adk/tool/mcptoolset"
    "google.golang.org/adk/agent/llmagent"
    "google.golang.org/adk/tool"
)
```

## 🚀 Implementation Patterns

### 1. In-Memory MCP (Local Tools)
Useful for wrapping existing Go functions as MCP tools.

```go
clientTransport, serverTransport := mcp.NewInMemoryTransports()
server := mcp.NewServer(&mcp.Implementation{Name: "my_server", Version: "1.0.0"}, nil)

// register tool
mcp.AddTool(server, &mcp.Tool{Name: "my_tool", Description: "desc"}, myGoFunc)
server.Connect(ctx, serverTransport, nil)

mcpToolset, _ := mcptoolset.New(mcptoolset.Config{
    Transport: clientTransport,
})
```

### 2. Remote MCP (e.g., GitHub)
```go
transport := &mcp.StreamableClientTransport{
    Endpoint: "https://api.githubcopilot.com/mcp/",
    HTTPClient: authenticatedClient,
}
mcpToolset, _ := mcptoolset.New(mcptoolset.Config{
    Transport: transport,
})
```

## 🧪 Integration with Agent
```go
a, _ := llmagent.New(llmagent.Config{
    Name:     "mcp_agent",
    Model:    model,
    Toolsets: []tool.Toolset{mcpToolset},
})
```

## ⚠️ Critical Notes
- MCP tools are dynamic; use `mcptoolset.New` to wrap the transport.
- For local debugging, `mcp.NewInMemoryTransports()` is the fastest verified path.
- Always use non-interactive runners for automated testing.
