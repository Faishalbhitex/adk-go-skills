---
name: adk-go-tools
description: Expert guidance for implementing tools and Human-in-the-Loop (HITL) confirmations in ADK-Go.
---

# ADK-Go Tools & Confirmations

Use this skill to implement custom function tools and safety confirmations.

## 🛠️ Mandatory Imports

```go
import (
    "google.golang.org/adk/tool"
    "google.golang.org/adk/tool/toolconfirmation"
    "google.golang.org/adk/tool/functiontool"
    "google.golang.org/adk/agent/llmagent"
)
```

## 🚀 Implementation Patterns

### 1. Simple Function Tool
```go
myTool := functiontool.New("get_weather", "returns weather", func(ctx context.Context, input struct{ City string }) (string, error) {
    return "Sunny in " + input.City, nil
})
```

### 2. Tool with Confirmation (HITL)
Use `tool.WithConfirmation` to wrap any tool that requires user approval before execution.

```go
safeTool := tool.WithConfirmation(myTool, tool.ConfirmationConfig{
    Hint: "This tool will access your location. Do you approve?",
})

a, _ := llmagent.New(llmagent.Config{
    Tools: []tool.Tool{safeTool},
})
```

## 🧪 Handling Confirmation in Runner

When a tool requires confirmation, ADK emits a special event with name `adk_request_confirmation`.

1. **Detect**: Event has `FunctionCall.Name == "adk_request_confirmation"`.
2. **Respond**: You must send a `FunctionResponse` to the runner to approve/deny.

```go
// Response structure to approve
resp := &genai.Content{
    Role: "user",
    Parts: []*genai.Part{
        {
            FunctionResponse: &genai.FunctionResponse{
                Name: "adk_request_confirmation",
                Response: map[string]any{"confirmed": true},
            },
        },
    },
}
```

## 📂 References
- [Function Tools](./references/function-tools.md): Custom tool creation and schemas.
- [Artifacts & Memory](./references/artifact-memory.md): File management and long-term context.
- [Confirmation Workflow](./references/confirmation-workflow.md): Handling HITL / `adk_request_confirmation`.

## ⚠️ Critical Notes
- Tools wrapped with `WithConfirmation` will NOT execute until a `confirmed: true` response is received.
- Use `toolconfirmation.OriginalCallFrom` to inspect what the agent *intended* to do before confirming.
- `gemini-2.5-flash` is highly reliable for multi-step tool interactions.
