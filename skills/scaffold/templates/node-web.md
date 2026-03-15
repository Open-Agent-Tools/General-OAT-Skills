# React/Next.js Web Template

React/Next.js web application with TypeScript, Tailwind CSS, and app router.

## Directory Structure

```
{{PROJECT_NAME}}/
├── src/
│   ├── app/
│   │   ├── layout.tsx            # Root layout
│   │   ├── page.tsx              # Home page
│   │   └── globals.css           # Global styles with Tailwind
│   ├── components/
│   │   ├── ui/                   # Reusable UI components
│   │   │   └── button.tsx
│   │   └── layout/
│   │       └── header.tsx        # Layout components
│   ├── lib/
│   │   └── utils.ts              # Utility functions
│   └── types/
│       └── index.ts              # Shared TypeScript types
├── public/
│   └── favicon.ico
├── tests/
│   └── page.test.tsx
├── next.config.js
├── tailwind.config.ts
├── postcss.config.js
├── tsconfig.json
├── package.json
├── eslint.config.js
├── .github/
│   └── workflows/
│       └── ci.yml
├── .gitignore
├── .editorconfig
├── README.md
└── LICENSE
```

## Dependencies

- `next`
- `react`
- `react-dom`
- `tailwindcss`
- `postcss`
- `autoprefixer`
- `typescript` (dev)
- `@types/react` (dev)
- `@types/react-dom` (dev)
- `eslint` (dev)
- `vitest` (dev)
- `@testing-library/react` (dev)
