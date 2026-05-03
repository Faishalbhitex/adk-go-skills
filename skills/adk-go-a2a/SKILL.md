---
name: adk-go-a2a
description: Expert guidance for implementing Agent-to-Agent (A2A) communication in ADK-Go using JSON-RPC and Agent Cards.
---

# ADK-Go A2A (Agent-to-Agent)

Use this skill to expose agents as services or consume remote agents.

## 🛠️ Mandatory Imports

```go
import (
    "github.com/a2aproject/a2a-go/a2a"
    "github.com/a2aproject/a2a-go/a2asrv"
    "google.golang.org/adk/server/adka2a"
    "google.golang.org/adk/agent/remoteagent"
    "google.golang.org/adk/runner"
    "google.golang.org/adk/session"
)
```

## 🚀 Implementation Pattern

### 1. Exposing an Agent (Server)
```go
// 1. Create the Executor
executor := adka2a.NewExecutor(adka2a.ExecutorConfig{
    RunnerConfig: runner.Config{
        AppName:        "my-agent-service",
        Agent:          localAgent,
        SessionService: session.InMemoryService(),
    },
})

// 2. Setup Handlers
mux := http.NewServeMux()
requestHandler := a2asrv.NewHandler(executor)
mux.Handle("/invoke", a2asrv.NewJSONRPCHandler(requestHandler))

// 3. Expose Agent Card (.well-known/agent-card)
card := &a2a.AgentCard{
    Name:   "MyAgent",
    URL:    "http://localhost:8080/invoke",
    Skills: adka2a.BuildAgentSkills(localAgent),
}
mux.Handle(a2asrv.WellKnownAgentCardPath, a2asrv.NewStaticAgentCardHandler(card))
```

### 2. Consuming an Agent (Client)
```go
remoteAgent, err := remoteagent.NewA2A(remoteagent.A2AConfig{
    Name:            "RemoteWeatherAgent",
    AgentCardSource: "http://localhost:8080", // Root URL where card is exposed
})
```

## 🧪 Verification Steps
- Verify `.well-known/agent-card` returns valid JSON.
- Ensure `remoteagent.NewA2A` can fetch and parse the card.
- Use the linear runner to test remote agent calls.

## ⚠️ Critical Notes
- A2A relies on `json-rpc` for the invocation layer.
- `adka2a.BuildAgentSkills` is the helper to generate MCP-compatible skill metadata for the agent card.
