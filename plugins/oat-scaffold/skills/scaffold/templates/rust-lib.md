# Rust Library Template

Rust library with Cargo, tests, benchmarks, and CI.

## Directory Structure

```
{{PROJECT_NAME}}/
├── src/
│   ├── lib.rs                    # Library root with public API
│   └── core.rs                   # Core module implementation
├── tests/
│   └── integration_test.rs       # Integration tests
├── benches/
│   └── benchmark.rs              # Performance benchmarks
├── examples/
│   └── basic.rs                  # Usage example
├── Cargo.toml                    # Package metadata and dependencies
├── .github/
│   └── workflows/
│       └── ci.yml                # CI: clippy, test, fmt check
├── .gitignore
├── .editorconfig
├── README.md
├── LICENSE
└── CHANGELOG.md
```

## Dependencies

- `serde` (with `derive` feature, optional serialization)
- `thiserror` (error handling)
- `criterion` (dev, benchmarks)
