# Function Tools in ADK-Go

Function tools allow agents to execute Go code to perform tasks or retrieve data.

## 🛠️ Mandatory Imports

```go
import (
    "context"
    "google.golang.org/adk/tool"
    "google.golang.org/adk/tool/functiontool"
)
```

## 🚀 Creating a Tool

Use `functiontool.New` to wrap a Go function. The function must have the signature:
`func(tool.Context, TArgs) (TResults, error)`

### Simple Tool
```go
type WeatherArgs struct {
    City string `json:"city"`
}

getWeather, _ := functiontool.New(functiontool.Config{
    Name:        "get_weather",
    Description: "Returns the current weather for a city.",
}, func(ctx tool.Context, args WeatherArgs) (string, error) {
    return "Sunny in " + args.City, nil
})
```

### Tool with Confirmation (HITL)
Set `RequireConfirmation: true` in the config to force Human-in-the-Loop approval.

```go
deleteTool, _ := functiontool.New(functiontool.Config{
    Name:                "delete_resource",
    Description:         "Deletes a cloud resource.",
    RequireConfirmation: true, // Static requirement
}, deleteHandler)
```

### Dynamic Confirmation
Use `RequireConfirmationProvider` to decide at runtime based on arguments.

```go
transferTool, _ := functiontool.New(functiontool.Config{
    Name: "transfer_funds",
    RequireConfirmationProvider: func(args TransferArgs) bool {
        return args.Amount > 1000 // Only confirm large transfers
    },
}, transferHandler)
```

## ⚠️ Important Details
- **Schema Inference**: ADK automatically generates JSON schemas from your Go structs. Use `json` tags to control field names.
- **TResults**: If your function returns a simple type (string, int), ADK wraps it in `{"result": value}`.
- **Errors**: Returning an error from your handler will be passed back to the LLM for recovery or reporting.
