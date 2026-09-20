# Node.js Library Template

TypeScript library with modern tooling, ESM support, and npm publishing config.

## Directory Structure

```
{{PROJECT_NAME}}/
├── src/
│   ├── index.ts                   # Main entry point and exports
│   ├── core.ts                    # Core library functionality
│   └── utils.ts                   # Utility functions
├── tests/
│   └── core.test.ts              # Core module tests
├── dist/                          # Built output (gitignored)
├── tsconfig.json                  # TypeScript configuration
├── tsconfig.build.json           # Build-specific TS config
├── package.json                   # With main, types, exports fields
├── vitest.config.ts              # Test configuration
├── eslint.config.js              # ESLint flat config
├── .github/
│   └── workflows/
│       └── ci.yml                # CI: lint, test, build, publish
├── .gitignore
├── .editorconfig
├── README.md
├── LICENSE
└── CHANGELOG.md
```

## Dependencies

- `typescript` (dev)
- `vitest` (dev)
- `eslint` (dev)
- `prettier` (dev)
- `tsup` (dev, for building)
