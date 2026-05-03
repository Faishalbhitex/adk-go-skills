# Sessions & State Management

A Session tracks an individual conversation thread, its history, and its state.

## 🗄️ Session Service
Use `session.Service` to manage the lifecycle of conversations.

```go
import "google.golang.org/adk/session"

// 1. Create a service (InMemory for dev/testing)
service := session.InMemoryService()

// 2. Create or Get a session
resp, _ := service.Create(ctx, &session.CreateRequest{
    AppName: "my-app",
    UserID:  "user-123",
})
sess := resp.Session
```

## 💾 State Management
State is a key-value store tied to the session.

### Reading State
```go
val, err := sess.State().Get("my_key")
```

### Modifying State
State updates are typically signaled via `Event.Actions.StateDelta`. The `Runner` handles this automatically when tools or agents return state changes.

## 🔄 Lifecycle in Runner
1. **Runner.Run** is called with `sessionID`.
2. **Runner** retrieves the session from `SessionService`.
3. **Agent** executes, potentially reading from `session.Events()` for history.
4. **Resulting Events** are appended back to the session via `service.AppendEvent`.
5. **SessionState** is updated based on the deltas in the appended events.

## ⚠️ Key Rule
Never modify the `Session` object directly in concurrent code. Always use the `SessionService` methods or rely on the `Runner`'s automated event processing.
