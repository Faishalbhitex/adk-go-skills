# Events: The Information Unit of ADK-Go

Events represent every significant occurrence during an interaction lifecycle.

## 📦 Event Structure
In Go, events are represented by `session.Event`.

```go
type Event struct {
    model.LLMResponse // Embedded: Content, Partial, TurnComplete

    Author       string    // 'user' or agent name
    InvocationID string    // ID for the whole interaction run
    ID           string    // Unique ID for this specific event
    Timestamp    time.Time // Creation time
    Actions      EventActions // Side-effects & control flow
}
```

## 🔍 Event Identification

### Identifying Content Type
```go
func handleEvent(e *session.Event) {
    if e.Content == nil { return }
    
    for _, part := range e.Content.Parts {
        if part.FunctionCall != nil {
            // Model wants to use a tool
        }
        if part.FunctionResponse != nil {
            // Tool execution result
        }
        if part.Text != "" {
            // Conversational response
        }
    }
}
```

## ⚡ Side Effects (Actions)
`event.Actions` signal changes that should be applied to the session.

- **StateDelta**: `map[string]any` representing modified state keys.
- **ArtifactDelta**: `map[string]artifact.Artifact` representing saved files.
- **TransferToAgent**: String name of the next agent to run.
- **Escalate**: Boolean signal to exit a loop or delegate to parent.

## 🏁 Final Response
Use `event.IsFinalResponse()` (or equivalent logic) to determine if an event should be displayed to the end-user.

Criteria for Final Response:
1. No pending function calls.
2. `Partial` is `false` (stream is complete).
3. `TurnComplete` is `true`.
