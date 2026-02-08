# Bun Dependency Setup

This document contains the Bun commands required to install the listed dependencies and devDependencies.

---

## Dependencies

```bash
bun add @eslint/eslintrc@3.2.0
```

<!-- Copy button for Markdown renderers that support HTML -->
<button onclick="navigator.clipboard.writeText('bun add @eslint/eslintrc@3.2.0')">Copy command</button>

---

## Dev Dependencies

```bash
bun add -d \
  eslint@9.18.0 \
  @eslint/compat@1.2.7 \
  eslint-config-next@15.2.4 \
  eslint-config-prettier@10.1.1 \
  eslint-plugin-prettier@5.2.3 \
  prettier@3.5.3 \
  prettier-plugin-tailwindcss@0.6.11 \
  @ianvs/prettier-plugin-sort-imports@4.4.1 \
  @types/eslint__eslintrc@2.1.2
```
<!-- Copy button for Markdown renderers that support HTML -->
<button onclick="navigator.clipboard.writeText('bun add -d eslint@9.18.0 @eslint/compat@1.2.7 eslint-config-next@15.2.4 eslint-config-prettier@10.1.1 eslint-plugin-prettier@5.2.3 prettier@3.5.3 prettier-plugin-tailwindcss@0.6.11 @ianvs/prettier-plugin-sort-imports@4.4.1 @types/eslint__eslintrc@2.1.2')">Copy</button>

---

## ESLint Configuration (`eslint.config.mjs`)

```js
import { dirname, resolve } from "path"
import { fileURLToPath } from "url"

import { includeIgnoreFile } from "@eslint/compat"
import { FlatCompat } from "@eslint/eslintrc"

const __filename = fileURLToPath(import.meta.url)
const __dirname = dirname(__filename)
const gitignorePath = resolve(__dirname, ".gitignore")

const compat = new FlatCompat({
  baseDirectory: __dirname,
})

const eslintConfig = [
  includeIgnoreFile(gitignorePath),
  ...compat.extends("next/core-web-vitals", "next/typescript", "plugin:prettier/recommended"),
  {
    rules: {
      "import/consistent-type-specifier-style": ["error", "prefer-top-level"],
      "@typescript-eslint/consistent-type-imports": "error",
      "@typescript-eslint/no-empty-object-type": "off",
      "@typescript-eslint/ban-ts-comment": "off",
      "@typescript-eslint/no-unused-vars": [
        "error",
        {
          argsIgnorePattern: "^_",
          varsIgnorePattern: "^_",
          caughtErrorsIgnorePattern: "^_",
        },
      ],
    },
  },
]

export default eslintConfig
```

---

## Prettier Configuration (`prettier.config.mjs`)

```js
/** @type {import('prettier').Config} */
const config = {
  plugins: ["prettier-plugin-tailwindcss", "@ianvs/prettier-plugin-sort-imports"],
  semi: false,
  singleQuote: false,
  trailingComma: "es5",
  printWidth: 80,
  tabWidth: 2,
  bracketSpacing: true,
  arrowParens: "always",
  endOfLine: "lf",
  tailwindStylesheet: "./src/app/globals.css",
  tailwindConfig: "./tailwind.config.ts",
  tailwindFunctions: ["cn", "clsx"],
  importOrder: [
    "<BUILTIN_MODULES>",
    "",
    "^(react/(.*)$)|^(react$)",
    "^(react-dom/(.*)$)|^(react-dom$)",
    "^(next/(.*)$)|^(next$)",
    "<THIRD_PARTY_MODULES>",
    "^(lucide-react/(.*)$)|^(lucide-react$)",
    "^(react-icons/(.*)$)|^(react-icons$)",
    "",
    ".css$",
    "",
    "<TYPES>^(node:)",
    "<TYPES>",
    "<TYPES>^[.]",
    "/types(.*)$",
    "",
    "/(_data|data)/(.*)$",
    "",
    "/(_schemas|schemas)/(.*)$",
    "",
    "/constants/(.*)$",
    "/configs/(.*)$",
    "/lib/(.*)$",
    "",
    "/(_hooks|hooks)/(.*)$",
    "/(_contexts|contexts)/(.*)$",
    "/(_providers|providers)/(.*)$",
    "^@/components/ui/(.*)$",
    "/(_components|components)/(.*)$",
    "[.]",
  ],
  importOrderParserPlugins: ["typescript", "jsx", "decorators-legacy"],
  importOrderTypeScriptVersion: "5.0.0",
  importOrderCaseSensitive: true,
}

export default config
```

---

## Notes

- Uses exact versions as specified
- Compatible with Bun
- ESLint Flat Config + Prettier fully wired
- Tailwind class sorting enabled via Prettier plugin
