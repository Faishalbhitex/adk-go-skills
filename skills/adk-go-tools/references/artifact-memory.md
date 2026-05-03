# Artifacts & Memory Tools

Use these tools to enable agents to retrieve and manage long-term context and files.

## 📦 Artifacts (File Management)

Artifacts allow agents to "see" and interact with files (images, PDFs, text) stored in a session.

### Usage
```go
import "google.golang.org/adk/tool/loadartifactstool"

// Add to agent config
Tools: []tool.Tool{
    loadartifactstool.New(),
},
```

### Saving Artifacts (Programmatic)
To make files available to the agent, save them to the `ArtifactService`:

```go
imageBytes, _ := os.ReadFile("animal_picture.png")
artifactService.Save(ctx, &artifact.SaveRequest{
    AppName:   appName,
    UserID:    userID,
    SessionID: sessionID,
    FileName:  "animal_picture.png",
    Part:      genai.NewPartFromBytes(imageBytes, "image/png"),
})
```

## 🧠 Memory (Context Retrieval)

Memory tools allow agents to search through history from *other* sessions of the same user.

### Usage
```go
import (
    "google.golang.org/adk/tool/loadmemorytool"
    "google.golang.org/adk/tool/preloadmemorytool"
)

// Add to agent config
Tools: []tool.Tool{
    preloadmemorytool.New(), // Automatically injects relevant context
    loadmemorytool.New(),    // Allows agent to search explicitly
},
```

### Populating Memory
The `MemoryService` typically indexes events from sessions.

```go
// Add a completed session to memory for future retrieval
memoryService.AddSessionToMemory(ctx, previousSession)
```

## ⚠️ Best Practices
- **Preload Memory**: Always include `preloadmemorytool` to reduce agent effort.
- **Artifact Formats**: Gemini handles common MIME types (image/png, image/jpeg, application/pdf) natively via `loadartifactstool`.
