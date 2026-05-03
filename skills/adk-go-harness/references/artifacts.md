# Artifacts: File Management in ADK-Go

Artifacts are files (images, documents, etc.) associated with a conversation session.

## 🛠️ Artifact Service
Use `artifact.Service` to store and retrieve files.

```go
import "google.golang.org/adk/artifact"

// 1. Create service (InMemory for dev/testing)
service := artifact.InMemoryService()

// 2. Save an artifact
imageBytes, _ := os.ReadFile("photo.png")
_, err := service.Save(ctx, &artifact.SaveRequest{
    AppName:   "my-app",
    UserID:    "user-1",
    SessionID: "sess-123",
    FileName:  "photo.png",
    Part:      genai.NewPartFromBytes(imageBytes, "image/png"),
})
```

## 🔍 Accessing Artifacts in Agents

### From Code (InvocationContext)
```go
func myAgent(ctx agent.InvocationContext) {
    // List artifacts
    list, _ := ctx.Artifacts().List(ctx, "photo.png")
    
    // Read content
    art, _ := ctx.Artifacts().Get(ctx, "photo.png")
}
```

### From Tools (load_artifacts)
The built-in `loadartifactstool` allows the LLM to retrieve artifacts by name.

```go
import "google.golang.org/adk/tool/loadartifactstool"

// Add to agent config
Tools: []tool.Tool{
    loadartifactstool.New(),
},
```

## 🏗️ Storage Backends
- **InMemoryService**: Default for development.
- **GCSArtifactService**: Stores files in Google Cloud Storage for production.

## ⚠️ Important
- Artifacts are scoped to a specific `SessionID`.
- When using `gemini-2.5-flash`, images and PDFs provided via `load_artifacts` are automatically processed by the model's multimodal vision capabilities.
