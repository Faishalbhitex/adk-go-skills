---
name: adk-go-skills
description: Expert guidance for implementing SkillToolset in ADK-Go, allowing agents to dynamically load instructions (SKILL.md) and resources.
---

# ADK-Go SkillToolset

Use this skill to extend agent capabilities using modular "Skills" (folders with instructions and assets).

## 🛠️ Mandatory Imports

```go
import (
    "os"
    "google.golang.org/adk/tool"
    "google.golang.org/adk/tool/skilltoolset"
    "google.golang.org/adk/tool/skilltoolset/skill"
    "google.golang.org/adk/agent/llmagent"
)
```

## 🚀 Implementation Pattern

1. **Folder Structure**: Each skill MUST have a `SKILL.md` with YAML frontmatter.
   ```
   my-skills/
   └── hello/
       ├── SKILL.md
       └── references/ (optional)
   ```

2. **Initialize Source & Toolset**:
   ```go
   // Source requires an fs.FS (e.g., os.DirFS)
   skillSource := skill.NewFileSystemSource(os.DirFS("./my-skills"))

   ts, err := skilltoolset.New(ctx, skilltoolset.Config{
       Source: skillSource,
   })
   
   // Add to llmagent.Config
   a, err := llmagent.New(llmagent.Config{
       Name:     "skills_agent",
       Model:    model,
       Toolsets: []tool.Toolset{ts},
   })
   ```

## 🧪 Linear Debugging (Runner)

Avoid `launcher` for automated tests/agents. Use the internal runner pattern:

```go
import (
    "google.golang.org/adk/runner"
    "google.golang.org/adk/session"
    "google.golang.org/adk/agent"
    "google.golang.org/genai"
)

// Linear execution flow
r, _ := runner.New(runner.Config{
    AppName: "test-app",
    Agent:   a,
    SessionService: session.InMemoryService(),
    AutoCreateSession: true,
})

msg := &genai.Content{Role: "user", Parts: []*genai.Part{{Text: "load skill hello"}}}
for event, err := range r.Run(ctx, "user-1", "sess-1", msg, agent.RunConfig{StreamingMode: agent.StreamingModeSSE}) {
    // Process events (see .user/adk-components/events.md)
}
```

## ⚠️ Critical Notes
- `skill.NewFileSystemSource` returns a single value (no error).
- `Toolsets` in `llmagent.Config` is a slice: `[]tool.Toolset{ts}`.
- Always use `gemini-2.5-flash` or newer for best tool-calling performance.
