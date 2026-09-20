# Go CLI Template

Go CLI application with Cobra, structured commands, and Go modules.

## Directory Structure

```
{{PROJECT_NAME}}/
├── cmd/
│   ├── root.go                   # Root command with Cobra
│   └── version.go                # Version command
├── internal/
│   ├── core/
│   │   └── core.go               # Core business logic
│   └── config/
│       └── config.go             # Configuration management
├── pkg/
│   └── utils/
│       └── utils.go              # Shared utility functions
├── main.go                        # Entry point
├── go.mod                        # Go module definition
├── go.sum
├── Makefile                      # Build, test, lint targets
├── .github/
│   └── workflows/
│       └── ci.yml                # CI: vet, test, build
├── .gitignore
├── .editorconfig
├── README.md
├── LICENSE
└── CHANGELOG.md
```

## Dependencies

- `github.com/spf13/cobra` (CLI framework)
- `github.com/spf13/viper` (configuration)
