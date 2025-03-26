# CraftzCode Turborepo Stack Guide

A comprehensive guide to setting up a modern full-stack development environment using Turborepo.

## Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Setup Instructions](#setup-instructions)
  - [1. Create Turborepo Project](#1-create-turborepo-project)
  - [2. Remove Boilerplates](#2-remove-boilerplates)
  - [3. GitHub Repository Setup](#3-github-repository-setup)
  - [4. Initial Configuration](#4-initial-configuration)
  - [5. Shared Configuration Setup](#5-shared-configuration-setup)

## Overview

This guide walks you through setting up a modern monorepo structure using Turborepo, with shared configurations for multiple applications. The stack includes Next.js for web applications, Expo for mobile development, and shared packages for API, database, authentication, and UI components.

## Tech Stack

| Category            | Technology                |
| ------------------- | ------------------------- |
| **Framework**       | Next.js 15, React 19      |
| **API**             | tRPC v11, Tanstack Query  |
| **Database**        | Drizzle ORM, PostgreSQL   |
| **Authentication**  | Better-Auth               |
| **Styling**         | Tailwind CSS, Shadcn UI   |
| **Mobile**          | Expo SDK 51, React Native |
| **Build System**    | Turborepo                 |
| **Package Manager** | Bun                       |

## Project Structure

```
apps
  ├─ docs                           # Documentation site
  |   ├─ Next.js 15
  |   ├─ React 19
  |   ├─ Tailwind CSS
  |   └─ E2E Typesafe API Server & Client
  ├─ web                            # Main web application
  |   ├─ Next.js 15
  |   ├─ React 19
  |   ├─ Tailwind CSS
  |   └─ E2E Typesafe API Server & Client
  └─ mobile                         # Mobile application
      ├─ Expo SDK 51
      ├─ React Native using React 18
      ├─ Navigation using Expo Router
      ├─ Tailwind using NativeWind
      └─ Typesafe API calls using tRPC
packages
  ├─ api                            # Shared API package
  |   └─ tRPC v11 router definition
  ├─ auth                           # Authentication package
  |   └─ Better-Auth
  ├─ db                             # Database package
  |   └─ Typesafe db calls using Drizzle & Neon Tech
  └─ ui                             # Shared UI components
      └─ UI package for the webapp using shadcn-ui
config
  ├─ eslint-config                  # Shared ESLint config
  │   └─ Shared, fine-grained ESLint presets
  ├─ prettier-config                # Shared Prettier config
  │   └─ Shared Prettier configuration
  ├─ tailwind-config                # Shared Tailwind config
  │   └─ Shared Tailwind CSS configuration
  └─ typescript-config              # Shared TypeScript config
      └─ Shared TSConfig to extend from
```

## Setup Instructions

### 1. Create Turborepo Project

Start by creating a new Turborepo project and setting up the package manager:

```bash
# Create a new Turborepo project
pnpm dlx create-turbo@latest craftzcode-stack

# When prompted, select Bun as the package manager
# Update all dependencies in the workspace
bun update
```

### 2. Remove Boilerplates

Clean up the boilerplate code from the generated project:

```bash
# Delete unnecessary files in both docs and web apps
# - page.module.css
# - All code inside page.tsx

# Commit your changes
git commit -m "refactor(docs|web): delete boilerplates"
```

### 3. GitHub Repository Setup

Create and configure a GitHub repository for your project:

```bash
# Create a new repository on GitHub
# Add the remote to your local repository
git remote add origin <your-github-repo-url>

# Push the main branch
git push -u origin main
```

### 4. Initial Configuration

Set up initial project configurations:

#### 4.1 Cursor Rules

If you're using Cursor AI editor, add custom rules:

```bash
# Create directory for Cursor rules
mkdir -p .cursor/rules

# Add your custom rules files
# Copy rules to .cursor/rules/{rules}.mdc

# Commit your changes
git commit -m "feat(cursor): add custom rules for Cursor AI Editor"
```

#### 4.2 Rename Package Imports

Update the package name prefix throughout the project:

```bash
# Find and replace all instances of @repo with @craftzcode
# This ensures imports will use your organization's name (e.g., @craftzcode/ui)

# Commit your changes
git commit -m "refactor(alias): rename @repo to @craftzcode"
```

#### 4.3 Configure TypeScript & ESLint

Move and configure TypeScript and ESLint configurations:

```bash
# Create config folder in project root
mkdir -p config

# Move existing configs into the config folder
mv typescript-config config/
mv eslint-config config/

# Update root package.json to include config in workspaces
# Add "config/*" to the workspaces array

# Reinstall packages
bun install

# Commit your changes
git commit -m "chore(config): move typescript and eslint configs to config folder"
```

#### 4.4 Create Git Branch and Pull Request

```bash
# Create feature branch
git checkout -b infrastructure/chore/1-cursor-alias-config

# Create pull request with title:
# "chore(infrastructure): add cursor rules, update aliases, and reorganize configs"
```

### 5. Shared Configuration Setup

Set up shared configurations for Prettier, Tailwind CSS, and Shadcn UI.

#### 5.1 File Structure

Ensure your project has the following file structure:

```
craftzcode-stack/
├── config/
│   ├── prettier-config/
│   │   ├── index.js
│   │   └── package.json
│   └── tailwind-config/
│       ├── package.json
│       ├── postcss.config.mjs
│       └── style.css
├── packages/
│   └── ui/ (shared shadcn-ui package)
│       ├── package.json
│       ├── tsconfig.json
│       └── src/
│           ├── lib/
│           │   └── utils.ts
│           ├── components/ (all existing components here should be deleted)
│           └── hooks/ (if any)
├── turbo.json (with additional commands: ui-add & clean)
└── package.json (root, with shared scripts)
```

#### 5.2 Prettier Configuration

Create and configure the shared Prettier configuration:

```bash
# Create prettier-config folder
mkdir -p config/prettier-config

# Create package.json file in config/prettier-config
# Add the package.json content as shown below
```

**config/prettier-config/package.json**:

```json
{
  "name": "@craftzcode/prettier-config",
  "version": "0.0.0",
  "private": true,
  "type": "module",
  "exports": {
    ".": "./index.js"
  },
  "scripts": {
    "lint": "next lint --max-warnings 0",
    "clean": "git clean -xdf .cache .turbo node_modules"
  },
  "devDependencies": {
    "@craftzcode/eslint-config": "*"
  },
  "prettier": "@craftzcode/prettier-config"
}
```

```bash
# Install Prettier and plugins
cd config/prettier-config
bun add -D prettier @ianvs/prettier-plugin-sort-imports prettier-plugin-tailwindcss
cd ../..

# Create index.js file with Prettier configuration
# Add the configuration content as shown below
```

**config/prettier-config/index.js**:

```js
/** @typedef {import("prettier").Config} PrettierConfig */
/** @typedef {import("prettier-plugin-tailwindcss").PluginOptions} TailwindConfig */
/** @typedef {import("@ianvs/prettier-plugin-sort-imports").PluginConfig} SortImportsConfig */

/** @type {PrettierConfig|SortImportsConfig|TailwindConfig} */
const config = {
  arrowParens: "avoid",
  bracketSpacing: true,
  endOfLine: "lf",
  jsxSingleQuote: true,
  singleQuote: true,
  semi: false,
  tabWidth: 2,
  trailingComma: "none",
  printWidth: 100,
  proseWrap: "preserve",
  quoteProps: "as-needed",
  plugins: [
    "@ianvs/prettier-plugin-sort-imports",
    "prettier-plugin-tailwindcss",
  ],
  importOrder: [
    // React imports
    "^(react/(.*)$)|^(react$)",
    "",
    // Next.js imports
    "^(next/(.*)$)|^(next$)",
    "",
    // React Native imports
    "^(react-native(.*)$)",
    // Expo imports
    "^(expo(.*)$)|^(expo$)",
    "",
    // Third-party modules
    "<THIRD_PARTY_MODULES>",
    "",
    // tRPC and TanStack libraries
    "^(@trpc/(.*)$)|^(@trpc$)|^(@tanstack/(.*)$)|^(@tanstack$)",
    "",
    // Database-related imports (Drizzle ORM)
    "^(drizzle-orm/(.*)$)|^(drizzle-orm$)|^(@/db/(.*)$)|^(@/db$)",
    "",
    // Project API modules
    "^@@craftzcode/api/(.*)$",
    "",
    // Project DB modules
    "^@@craftzcode/db/(.*)$",
    "",
    // Project auth modules
    "^@@craftzcode/auth/(.*)$",
    "",
    // Project module imports
    "@/modules(.*)$",
    "",
    // Project feature imports
    "^@/features/(.*)$",
    "",
    // Project utility libraries
    "^@/lib/(.*)$",
    "",
    // Project hooks
    "^@/hooks/(.*)$",
    "",
    // Form-related libraries
    "react-hook-form$",
    "zod$",
    "@hookform$",
    "",
    // UI components
    "^@@craftzcode/ui/(.*)$",
    "^@/components/(.*)$",
    "",
    // Public assets
    "^@/public/(.*)$",
    "",
    // Relative imports
    "^[./]",
  ],
  importOrderSeparation: false,
  importOrderSortSpecifiers: true,
  importOrderBuiltinModulesToTop: true,
  importOrderParserPlugins: ["typescript", "jsx", "decorators-legacy"],
  importOrderMergeDuplicateImports: true,
  importOrderCombineTypeAndValueImports: true,
};

export default config;
```

```bash
# Commit changes
git commit -m "chore(prettier): add shared prettier config for all workspaces"
```

#### 5.3 Tailwind CSS Configuration

Create and configure the shared Tailwind CSS configuration:

```bash
# Create tailwind-config folder
mkdir -p config/tailwind-config

# Create package.json file in config/tailwind-config
# Add the package.json content as shown below
```

**config/tailwind-config/package.json**:

```json
{
  "name": "@craftzcode/tailwind-config",
  "version": "0.1.0",
  "private": true,
  "type": "module",
  "exports": {
    "./style.css": "./style.css",
    "./postcss.config.mjs": "./postcss.config.mjs"
  },
  "scripts": {
    "lint": "next lint --max-warnings 0",
    "clean": "git clean -xdf .cache .turbo node_modules"
  },
  "devDependencies": {
    "@craftzcode/eslint-config": "*",
    "@craftzcode/prettier-config": "*",
    "eslint": "^9.22.0"
  },
  "prettier": "@craftzcode/prettier-config"
}
```

```bash
# Install Tailwind and dependencies
cd config/tailwind-config
bun add -D tailwindcss @tailwindcss/postcss
bun add tw-animate-css
cd ../..

# Create PostCSS config file
# Add the configuration content as shown below
```

**config/tailwind-config/postcss.config.mjs**:

```js
const config = {
  plugins: ["@tailwindcss/postcss"],
};

export default config;
```

```bash
# Create style.css file
# Copy content from an existing globals.css from a Next.js project with shadcn or copy the style.css of this repo
# Also add this css as shown below to include the shadcn shared config in packages/ui
```

**config/tailwind-config/style.css**:

```css
@source '../../packages/ui';
```

```bash
# Commit changes
git commit -m "chore(tailwind): add shared tailwind css config for all workspaces"
```

#### 5.4 Shadcn UI Configuration

Set up shared Shadcn UI components:

```bash
# Clean up existing UI components
rm -rf packages/ui/src/components/*

# Update package.json in packages/ui
# Add the content as shown below
```

**packages/ui/package.json**:

```json
{
  "name": "@craftzcode/ui",
  "version": "0.0.0",
  "private": true,
  "exports": {
    "./lib/*": "./src/lib/*.ts",
    "./components/*": "./src/components/*.tsx",
    "./hooks/*": "./src/hooks/*.ts"
  },
  "scripts": {
    "ui-add": "bunx --bun shadcn@latest add",
    "postui-add": "prettier src --write --list-different",
    "generate:component": "turbo gen react-component",
    "lint": "eslint . --max-warnings 0",
    "clean": "git clean -xdf .cache .turbo node_modules",
    "check-types": "tsc --noEmit"
  },
  "devDependencies": {
    "@craftzcode/eslint-config": "*",
    "@craftzcode/typescript-config": "*",
    "@craftzcode/tailwind-config": "*",
    "@craftzcode/prettier-config": "*",
    "@turbo/gen": "^2.4.4",
    "@types/node": "^22.13.10",
    "@types/react": "19.0.10",
    "@types/react-dom": "19.0.4",
    "eslint": "^9.22.0",
    "typescript": "5.8.2"
  },
  "dependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0"
  },
  "prettier": "@craftzcode/prettier-config"
}
```

```bash
# Install shadcn dependencies
cd packages/ui
bun add class-variance-authority clsx tailwind-merge lucide-react
cd ../..

# Create or update tsconfig.json in packages/ui
# Add the content as shown below
```

**packages/ui/tsconfig.json**:

```json
{
  "extends": "@craftzcode/typescript-config/react-library.json",
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@craftzcode/ui/*": ["./src/*"]
    }
  },
  "include": ["."],
  "exclude": ["node_modules", "dist"]
}
```

```bash
# Create utils.ts in packages/ui/src/lib
mkdir -p packages/ui/src/lib
# Add the content as shown below
```

**packages/ui/src/lib/utils.ts**:

```ts
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

```bash
# Create components.json for shadcn configuration
# Add the content as shown below
```

**packages/ui/components.json**:

```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "",
    "css": "../../config/tailwind-config/style.css",
    "baseColor": "neutral",
    "cssVariables": true,
    "prefix": ""
  },
  "aliases": {
    "components": "@craftzcode/ui/components",
    "utils": "@craftzcode/ui/lib/utils",
    "ui": "@craftzcode/ui/components",
    "lib": "@craftzcode/ui/lib",
    "hooks": "@craftzcode/ui/hooks"
  },
  "iconLibrary": "lucide"
}
```

#### 5.5 Root Configuration Updates

Update the root package.json and turbo.json files:

```bash
# Add scripts to root package.json
# Update with the content shown below
```

**Root package.json scripts**:

```json
{
  "scripts": {
    "ui-add": "turbo run ui-add -F @craftzcode/ui --",
    "clean": "git clean -xdf node_modules",
    "clean:workspaces": "turbo run clean"
  }
}
```

```bash
# Update turbo.json with new tasks
# Add the content shown below
```

**turbo.json tasks**:

```json
{
  "tasks": {
    "ui-add": {
      "cache": false,
      "interactive": true
    },
    "clean": {
      "cache": false
    }
  }
}
```

```bash
# Commit shadcn configuration
git commit -m "chore(shadcn): remove original files in packages/ui and setup shadcn"

# Create feature branch and prepare pull request
git checkout -b infrastructure/chore/2-shared-config
# Create pull request with title:
# "chore(infrastructure): add shared prettier & tailwind configs and setup shadcn in packages/ui"
```

## Next Steps

After completing this setup, you'll have a fully configured Turborepo with shared configurations for all your applications and packages. You can now proceed to build your applications using these shared resources.

## Additional Resources

- [Turborepo Documentation](https://turbo.build/repo/docs)
- [Next.js Documentation](https://nextjs.org/docs)
- [tRPC Documentation](https://trpc.io/docs)
- [Drizzle ORM Documentation](https://orm.drizzle.team/docs/overview)
- [Shadcn UI Documentation](https://ui.shadcn.com/docs)
- [Expo Documentation](https://docs.expo.dev)
