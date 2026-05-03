---
name: adk-go-harness
description: Expert guidance for setting up the core ADK-Go harness (Runner, Session, and execution loop).
---

# ADK-Go Core Harness

Use this skill to configure the basic runtime environment for ADK agents.

## 🛠️ Mandatory Imports

```go
import (
    "google.golang.org/adk/runner"
    "google.golang.org/adk/session"
    "google.golang.org/adk/agent"
    "google.golang.org/genai"
)
```

## 🚀 Setup Pattern

### 1. Initialize Services
```go
// InMemory is recommended for labs and stateless services
sessService := session.InMemoryService()
```

### 2. Configure Runner
```go
r, err := runner.New(runner.Config{
    AppName:           "my-app",
    Agent:             rootAgent,
    SessionService:    sessService,
    AutoCreateSession: true, // Recommended for linear flows
})
```

### 3. Execution Loop (Linear)
Always use a linear loop when building coding agents or automated tools.

```go
msg := &genai.Content{
    Role: "user",
    Parts: []*genai.Part{{Text: "Hello"}},
}

cfg := agent.RunConfig{StreamingMode: agent.StreamingModeSSE}

for event, err := range r.Run(ctx, "user-id", "session-id", msg, cfg) {
    if err != nil {
        // Handle error
    }
    // Process event.Content or event.Actions
}
```

## 📂 Key Components
- **Session**: Stores conversation history and state.
- **Runner**: Orchestrates agent tree traversal and tool execution.
- **Event**: The universal message format (see `.user/adk-components/events.md`).

## 📂 References
- [Events](./references/events.md): Understanding the universal message format.
- [Sessions & State](./references/sessions.md): Conversation lifecycle and data persistence.
- [Artifacts](./references/artifacts.md): Managing files and multimodal assets.

## ⚠️ Critical Notes
- `r.Run` returns an iterator (`iter.Seq2`).
- Always check `event.Partial` if `StreamingModeSSE` is used.
- `AppName` in `runner.Config` is mandatory for session identification.
