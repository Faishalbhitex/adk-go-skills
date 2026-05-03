# Confirmation Workflow (HITL)

Human-in-the-Loop (HITL) allows you to pause agent execution and wait for human approval before running high-risk tools.

## 🛡️ Wrapping Tools
Use `tool.WithConfirmation` to enable HITL for any tool.

```go
safeTool := tool.WithConfirmation(myTool, tool.ConfirmationConfig{
    Hint: "Delete file 'data.csv'?",
})
```

## 🔄 Interaction Flow

1. **Agent Request**: The agent attempts to call `myTool`.
2. **ADK Intercept**: ADK sees the wrapper and instead emits a `FunctionCall` with:
   - Name: `adk_request_confirmation`
   - Args: `{"hint": "Delete file 'data.csv'?", "originalFunctionCall": {...}}`
3. **Frontend Action**: The application displays the hint to the user.
4. **User Response**: The application sends a `FunctionResponse` back to the agent:
   - Name: `adk_request_confirmation`
   - Response: `{"confirmed": true}` (or `false`)
5. **Execution**: If confirmed, ADK finally executes the original tool and returns its result to the agent.

## 💻 Implementation Example (Client Side)

```go
for event := range runner.Run(...) {
    for _, part := range event.Content.Parts {
        if part.FunctionCall != nil && part.FunctionCall.Name == "adk_request_confirmation" {
            // 1. Show UI to user
            approved := askUser(part.FunctionCall.Args["hint"])
            
            // 2. Send response in NEXT runner.Run call
            response := &genai.Content{
                Role: "user",
                Parts: []*genai.Part{{
                    FunctionResponse: &genai.FunctionResponse{
                        Name: "adk_request_confirmation",
                        Response: map[string]any{"confirmed": approved},
                        ID: part.FunctionCall.ID,
                    },
                }},
            }
            // ... re-run with this content
        }
    }
}
```

## ⚠️ Important
- The `ID` in `FunctionResponse` MUST match the `ID` from the `adk_request_confirmation` call.
- If `confirmed` is `false`, the agent receives an error indicating the tool was cancelled.
