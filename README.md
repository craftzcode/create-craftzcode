# create-craftzcode

A monorepo tech stack template built with Turborepo, Next.js, tRPC, Drizzle ORM, TanStack Query, shadcn/ui, and Tailwind CSS.

### Overview

This repository offers a comprehensive starting point for full-stack applications. It leverages modern technologies and best practices to streamline development in a monorepo environment.

### Tech Stack Summary

- Monorepo Manager: Turborepo
- Frontend: Next.js 15, React 19
- Mobile: Expo (React Native)
- API & Data Fetching: tRPC v11, TanStack Query
- Database ORM: Drizzle ORM (with Neon Tech integration)
- UI Components: shadcn/ui
- Styling: Tailwind CSS (NativeWind for mobile)
- Authentication: JWT Tokens and Session Management
- Config: ESLint, Prettier, and shared TypeScript configurations

### Stack Components

Below is an overview of the project structure and its key components:

```
.github
  └─ workflows
        └─ CI with pnpm cache setup
.vscode
  └─ Recommended extensions and settings for VSCode users
apps
  ├─ auth-proxy
  |   ├─ Nitro server to proxy OAuth requests in preview deployments
  |   └─ Uses Auth.js Core
  ├─ expo
  |   ├─ Expo SDK 51
  |   ├─ React Native using React 18
  |   ├─ Navigation using Expo Router
  |   ├─ Tailwind using NativeWind
  |   └─ Typesafe API calls using tRPC
  └─ next.js
      ├─ Next.js 14
      ├─ React 18
      ├─ Tailwind CSS
      └─ E2E Typesafe API Server & Client
packages
  ├─ api
  |   └─ tRPC v11 router definition
  ├─ auth
  |   └─ Authentication.
  ├─ db
  |   └─ Typesafe db calls using Drizzle & Neon Tech
  └─ ui
      └─ UI package for the webapp using shadcn-ui
config
  ├─ eslint
  │    └─ Shared, fine-grained ESLint presets
  ├─ prettier
  │    └─ Shared Prettier configuration
  ├─ tailwind
  │    └─ Shared Tailwind CSS configuration
  └─ typescript
       └─ Shared TSConfig to extend from
```

## Installation

There are two ways to initialize an app using the `create-craftzcode` starter.

### Option 1: Use This Repository as a Template

Run the following command:

```shell
pnpm dlx create-turbo@latest -e https://github.com/craftzcode/create-craftzcode
```

### Option 2: Start a New Turborepo Project

1. **Create a New Project**

```shell
pnpm dlx create-turbo@latest create-craftzcode
```

2. **Update Package Manager & Dependencies**

- **Update the Package Manager Version**
  - Open the root `package.json` and change the `"packageManager"` field from `"pnpm@latest"` to `"pnpm@latest"`.

    ```json
    {
        // ... other settings
        "packageManager": "pnpm@10.4.1" // or the latest version
        // ... other settings
    }
    ```

  - Reinstall Dependencies
    ```shell
    pnpm install
    ```

- **Update All Dependencies Across the Workspace**
  ```shell
  pnpm up --recursive
  ```

- Git Commit
  ```shell
  git commit -m "chore(deps): update dependencies across workspace and package manager"
  ```

3. **Delete Docs & Web Boilerplates**

-  Delete `page.module.css` and delete all code in `page.tsx` both `docs & web`
-  Git Commit
   ```shell
   git commit -m "refactor(docs|web): delete bolierplates"
   ```

4. **Add Cursor Rules**

-  Copy my `.cursor/rules/{rules}.mdc`
-  Git Commit
   ```shell
   git commit -m "chore(.cursor): add cursor rules"
   ```

5. **Set Up GitHub Repository**

- Create a new repository named `create-craftzcode` on GitHub
- Initialize and push your local repository:

  ```shell
  git remote add origin git@github.com:craftzcode/create-craftzcode.git
  git branch -M main
  git push -u origin main
  ```

6. **Rename Workspace & Configure TypeScript & ESLint**

- **Git Branch**
  - Create a new branch for renaming the workspace and configuring TypeScript & ESLint.
    ```shell
    git checkout -b frontend/feature/1-rename-and-config
    ```
  - Pull Request Title
    ```
    feat(frontend): rename workspace and configure TS/ESLint
    ```
- **Rename Package Imports**
  - Rename all instances of `@repo` to `@craftzcode` in the project. This ensures that imports will look like `@craftzcode/ui`.
  - Git Commit

    ```
    git commit -m "refactor: rename package namespace from @repo to @craftzcode"
    ```

- **Configure TypeScript & ESLint**
  - Create a new folder named `config` in the root of your project.
  - Move the `typescript-config` and `eslint-config` into the `config` folder.
  - Add the `config` folder to `pnpm-workspace.yaml`.
  - Reinstall packages
    ```shell
    pnpm install
    ```
  - Git Commit
    ```shell
    git commit -m "chore(config): setup config folder for TypeScript & ESLint"
    ```

7. **Setup Prettier, Tailwind CSS, and Shadcn UI Shared Configs**

- **Overview File Structure**
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
  │           └── hooks/  (if any)
  ├── turbo.json (with additional commands: ui-add & clean)
  └── package.json (root, with shared scripts)
  ```

- **Git Branch**
  - Create a new branch for setting up Prettier, Tailwind CSS, and Shadcn UI Shared Configs.
    ```shell
    git checkout -b frontend/feature/1-setup-prettier-tailwind-shadcn
    ```
  - Pull Request Title
    ```
    feat(frontend): setup Prettier, Tailwind CSS, and Shadcn UI Shared Configs
    ```
- **Create the Prettier Config Package**
  - Create a folder called `prettier-config` inside the `config` folder.
  - Inside `config/prettier-config`, create a `package.json` file with the following content.
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
        "@craftzcode/eslint-config": "workspace:*"
      },
      "prettier": "@craftzcode/prettier-config"
    }
    ```
    **Explanations**
      - `"exports"`
      
        This field defines the entry point of the package. When another package imports `@craftzcode/prettier-config`, it will resolve to the index.js file.
        
        **Example Usage**
        ```js
        import config from '@craftzcode/prettier-config'
        ```

      - `"scripts"`
        - `"lint": "next lint --max-warnings 0"`

          A shortcut command to run linting with strict settings.

        - `"clean": "git clean -xdf .cache .turbo node_modules"`
          
          A command to clean up caches and node modules for a fresh install.

      - `"devDependencies"`
      
        The workspace dependencies (`workspace:*`) ensure that shared configurations for ESLint, Prettier, Tailwind, and TypeScript are used from the monorepo. Even if ESLint or TypeScript are installed in a shared config, including them here ensures that this package can run its commands independently.

      - `"prettier": "@craftzcode/prettier-config"`

        Specifies that the package should use the shared Prettier configuration for consistent code formatting
    
  - Install Prettier and Plugins
    ```shell
    pnpm --filter @craftzcode/prettier-config add prettier @ianvs/prettier-plugin-sort-imports prettier-plugin-tailwindcss
    ```

    **Explanations**
      - `pnpm --filter`

        When you run a command with `pnpm --filter <package>`, it targets a specific package within your monorepo. For example, `pnpm --filter @craftzcode/prettier-config add ...` installs dependencies only in the `@craftzcode/prettier-config` package.

  - Inside the `config/prettier-config` folder, create an `index.js` file with the following content.

    ```js
    /** @typedef {import("prettier").Config} PrettierConfig */
    /** @typedef {import("prettier-plugin-tailwindcss").PluginOptions} TailwindConfig */
    /** @typedef {import("@ianvs/prettier-plugin-sort-imports").PluginConfig} SortImportsConfig */

    /** @type {PrettierConfig|SortImportsConfig|TailwindConfig} */
    const config = {
      arrowParens: 'avoid',
        bracketSpacing: true,
        endOfLine: 'lf',
        jsxSingleQuote: true,
        singleQuote: true,
        semi: false,
        tabWidth: 2,
        trailingComma: 'none',
        plugins: [
          '@ianvs/prettier-plugin-sort-imports',
          'prettier-plugin-tailwindcss'
        ],
        importOrder: [
          '^(react/(.*)$)|^(react$)',
          '',
          '^(next/(.*)$)|^(next$)',
          '',
          '^(react-native(.*)$)',
          '^(expo(.*)$)|^(expo$)',
          '',
          '<THIRD_PARTY_MODULES>',
          '',
          // First group: trpc and tanstack imports
          '^(@trpc/(.*)$)|^(@trpc$)|^(@tanstack/(.*)$)|^(@tanstack$)',
          '',
          // Second group: drizzle-orm and @/db imports
          '^(drizzle-orm/(.*)$)|^(drizzle-orm$)|^(@/db/(.*)$)|^(@/db$)',
          '',
          '^@craftzcode/api/(.*)$',
          '',
          '^@craftzcode/db/(.*)$',
          '',
          '^@craftzcode/auth/(.*)$',
          '',
          '@/modules(.*)$',
          '',
          '^@/features/(.*)$',
          '',
          '^@/lib/(.*)$',
          '',
          '^@/hooks/(.*)$',
          '',
          'react-hook-form$',
          'zod$',
          '@hookform$',
          '',
          '^@craftzcode/ui/(.*)$',
          '^@/components/(.*)$',
          '',
          '^@/public/(.*)$',
          '',
          '^[./]'
        ],
        importOrderSeparation: false,
        importOrderSortSpecifiers: true,
        importOrderBuiltinModulesToTop: true,
        importOrderParserPlugins: ['typescript','jsx','decorators-legacy'],
        importOrderMergeDuplicateImports: true,
        importOrderCombineTypeAndValueImports: true
      }

      export default config     
    
  - Git Commit
    ```shell
    git commit -m "feat(prettier-config): add shared Prettier configuration"
    ```

- **Create the Tailwind CSS Config Package**
  - Inside the `config` folder, create a new folder named `tailwind-config`.
  - Inside `config/tailwind-config`, create a `package.json` file with the following content.
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
        "@craftzcode/eslint-config": "workspace:*",
        "@craftzcode/prettier-config": "workspace:*",
        "eslint": "^9.22.0"
      },
      "prettier": "@craftzcode/prettier-config"
    }
    ```
    **Explanations**
      - `"exports"`

        This field defines subpath exports for the package. It allows other parts of the project to import these files directly.

        **Example Usage**

        ```js
        import "@craftzcode/tailwind-config/style.css"
        // or require('@craftzcode/tailwind-config/postcss.config.mjs')
        ```

      - `"scripts"`
        - `"lint": "next lint --max-warnings 0"`

          A shortcut command to run linting with strict settings.

        - `"clean": "git clean -xdf .cache .turbo node_modules"`
          
          A command to clean up caches and node modules for a fresh install.

      - `"devDependencies"`
      
        The workspace dependencies (`workspace:*`) ensure that shared configurations for ESLint, Prettier, Tailwind, and TypeScript are used from the monorepo. Even if ESLint or TypeScript are installed in a shared config, including them here ensures that this package can run its commands independently.

      - `"prettier": "@craftzcode/prettier-config"`

        Specifies that the package should use the shared Prettier configuration for consistent code formatting.


  - Install Tailwind Dependencies
    ```shell
    pnpm --filter @craftzcode/tailwind-config add postcss @tailwindcss/postcss tailwindcss tailwindcss-animate
    ```

    **Explanations**
      - `pnpm --filter`

        When you run a command with `pnpm --filter <package>`, it targets a specific package within your monorepo. For example, `pnpm --filter @craftzcode/tailwind-config add ...` installs dependencies only in the `@craftzcode/tailwind-config` package.
  
  - Inside the `config/tailwind-config` folder, create a file named `postcss.config.mjs` with the following content.
    ```js
    const config = {
      plugins: {
        '@tailwindcss/postcss': {}
      }
    }

    export default config
    ```

    **Explanations**
    
    PostCSS is a tool for transforming CSS with JavaScript. In this configuration, it’s set up to process Tailwind CSS using the official PostCSS plugin from Tailwind.

  - Inside the same `config/tailwind-config` folder, create a file named `style.css` with the following content.
    ```css
    @import 'tailwindcss';

    /*
      ---break---
    */
    @plugin 'tailwindcss-animate';
    /*
      ---break---
    */
    @custom-variant dark (&:is(.dark *));

    @theme {
      --color-background: var(--background);
      --color-foreground: var(--foreground);
    }

    /*
      The default border color has changed to `currentColor` in Tailwind CSS v4,
      so we've added these compatibility styles to make sure everything still
      looks the same as it did with Tailwind CSS v3.

      If we ever want to remove these styles, we need to add an explicit border
      color utility to any element that depends on these defaults.
    */
    @layer base {
      *,
      ::after,
      ::before,
      ::backdrop,
      ::file-selector-button {
        border-color: var(--color-gray-200, currentColor);
      }
    }
    /*
      ---break---
    */
    :root {
      --background: hsl(0 0% 100%);
      --foreground: hsl(0 0% 3.9%);
      --card: hsl(0 0% 100%);
      --card-foreground: hsl(0 0% 3.9%);
      --popover: hsl(0 0% 100%);
      --popover-foreground: hsl(0 0% 3.9%);
      --primary: hsl(0 0% 9%);
      --primary-foreground: hsl(0 0% 98%);
      --secondary: hsl(0 0% 96.1%);
      --secondary-foreground: hsl(0 0% 9%);
      --muted: hsl(0 0% 96.1%);
      --muted-foreground: hsl(0 0% 45.1%);
      --accent: hsl(0 0% 96.1%);
      --accent-foreground: hsl(0 0% 9%);
      --destructive: hsl(0 84.2% 60.2%);
      --destructive-foreground: hsl(0 0% 98%);
      --border: hsl(0 0% 89.8%);
      --input: hsl(0 0% 89.8%);
      --ring: hsl(0 0% 3.9%);
      --chart-1: hsl(12 76% 61%);
      --chart-2: hsl(173 58% 39%);
      --chart-3: hsl(197 37% 24%);
      --chart-4: hsl(43 74% 66%);
      --chart-5: hsl(27 87% 67%);
      --radius: 0.6rem;
      --sidebar: hsl(0 0% 98%);
      --sidebar-foreground: hsl(240 5.3% 26.1%);
      --sidebar-primary: hsl(240 5.9% 10%);
      --sidebar-primary-foreground: hsl(0 0% 98%);
      --sidebar-accent: hsl(240 4.8% 95.9%);
      --sidebar-accent-foreground: hsl(240 5.9% 10%);
      --sidebar-border: hsl(220 13% 91%);
      --sidebar-ring: hsl(217.2 91.2% 59.8%);
    }
    /*
      ---break---
    */
    .dark {
      --background: hsl(0 0% 3.9%);
      --foreground: hsl(0 0% 98%);
      --card: hsl(0 0% 3.9%);
      --card-foreground: hsl(0 0% 98%);
      --popover: hsl(0 0% 3.9%);
      --popover-foreground: hsl(0 0% 98%);
      --primary: hsl(0 0% 98%);
      --primary-foreground: hsl(0 0% 9%);
      --secondary: hsl(0 0% 14.9%);
      --secondary-foreground: hsl(0 0% 98%);
      --muted: hsl(0 0% 14.9%);
      --muted-foreground: hsl(0 0% 63.9%);
      --accent: hsl(0 0% 14.9%);
      --accent-foreground: hsl(0 0% 98%);
      --destructive: hsl(0 62.8% 30.6%);
      --destructive-foreground: hsl(0 0% 98%);
      --border: hsl(0 0% 14.9%);
      --input: hsl(0 0% 14.9%);
      --ring: hsl(0 0% 83.1%);
      --chart-1: hsl(220 70% 50%);
      --chart-2: hsl(160 60% 45%);
      --chart-3: hsl(30 80% 55%);
      --chart-4: hsl(280 65% 60%);
      --chart-5: hsl(340 75% 55%);
      --sidebar: hsl(240 5.9% 10%);
      --sidebar-foreground: hsl(240 4.8% 95.9%);
      --sidebar-primary: hsl(224.3 76.3% 48%);
      --sidebar-primary-foreground: hsl(0 0% 100%);
      --sidebar-accent: hsl(240 3.7% 15.9%);
      --sidebar-accent-foreground: hsl(240 4.8% 95.9%);
      --sidebar-border: hsl(240 3.7% 15.9%);
      --sidebar-ring: hsl(217.2 91.2% 59.8%);
    }
    /*
      ---break---
    */
    @theme inline {
      --color-background: var(--background);
      --color-foreground: var(--foreground);
      --color-card: var(--card);
      --color-card-foreground: var(--card-foreground);
      --color-popover: var(--popover);
      --color-popover-foreground: var(--popover-foreground);
      --color-primary: var(--primary);
      --color-primary-foreground: var(--primary-foreground);
      --color-secondary: var(--secondary);
      --color-secondary-foreground: var(--secondary-foreground);
      --color-muted: var(--muted);
      --color-muted-foreground: var(--muted-foreground);
      --color-accent: var(--accent);
      --color-accent-foreground: var(--accent-foreground);
      --color-destructive: var(--destructive);
      --color-destructive-foreground: var(--destructive-foreground);
      --color-border: var(--border);
      --color-input: var(--input);
      --color-ring: var(--ring);
      --color-chart-1: var(--chart-1);
      --color-chart-2: var(--chart-2);
      --color-chart-3: var(--chart-3);
      --color-chart-4: var(--chart-4);
      --color-chart-5: var(--chart-5);
      --radius-sm: calc(var(--radius) - 4px);
      --radius-md: calc(var(--radius) - 2px);
      --radius-lg: var(--radius);
      --radius-xl: calc(var(--radius) + 4px);
      --color-sidebar-ring: var(--sidebar-ring);
      --color-sidebar-border: var(--sidebar-border);
      --color-sidebar-accent-foreground: var(--sidebar-accent-foreground);
      --color-sidebar-accent: var(--sidebar-accent);
      --color-sidebar-primary-foreground: var(--sidebar-primary-foreground);
      --color-sidebar-primary: var(--sidebar-primary);
      --color-sidebar-foreground: var(--sidebar-foreground);
      --color-sidebar: var(--sidebar);
      --animate-accordion-down: accordion-down 0.2s ease-out;
      --animate-accordion-up: accordion-up 0.2s ease-out;
    /*
      ---break---
    */
    @keyframes accordion-down {
      from {
        height: 0;
      }
      to {
        height: var(--radix-accordion-content-height);
      }
    }
    /*
      ---break---
    */
    @keyframes accordion-up {
      from {
        height: var(--radix-accordion-content-height);
      }
      to {
        height: 0;
      }
    }
    /*
      ---break---
    */
    @layer base {
      * {
        @apply border-border outline-ring/50;
      }
      body {
        @apply bg-background text-foreground;
      }
    }
    ```

  - Git Commit
    ```shell
    git commit -m "feat(tailwind-config): add shared Tailwind configuration with PostCSS and style setup"
    ```

- **Setup Shared shadcn-ui**
  - Delete all files inside the `packages/ui/src/components` folder to prepare for shadcn-ui’s generated components.
  - Replace the contents of `packages/ui/package.json` with the following code
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
        "ui-add": "pnpm dlx shadcn@canary add",
        "postui-add": "prettier src --write --list-different",
        "generate:component": "turbo gen react-component",
        "lint": "eslint . --max-warnings 0",
        "clean": "git clean -xdf .cache .turbo node_modules",
        "check-types": "tsc --noEmit",
      },
      "dependencies": {
        "react": "^19.0.0",
        "react-dom": "^19.0.0"
      },
      "devDependencies": {
        "@craftzcode/eslint-config": "workspace:*",
        "@craftzcode/prettier-config": "workspace:*",
        "@craftzcode/tailwind-config": "workspace:*",
        "@craftzcode/typescript-config": "workspace:*",
        "@turbo/gen": "^2.4.2",
        "@types/node": "^22.13.5",
        "@types/react": "19.0.8",
        "@types/react-dom": "19.0.3",
        "eslint": "^9.21.0",
        "typescript": "5.7.3"
      },
      "prettier": "@craftzcode/prettier-config"
    }
    ```
    **Explanations**
      - `"exports"`

        Defines the public API of the package. For example, importing from `@craftzcode/ui/lib/someUtil` will resolve to the corresponding TypeScript file in `src/lib`.

        **Example Usage**
        ```js
        import { cn } from '@craftzcode/ui/lib/utils'
        ```

      - `"scripts"`
        - `"ui-add": "pnpm dlx shadcn@canary add"`

          A command to add a new UI component using Shadcn UI.

        - `"postui-add": "prettier src --write --list-different"`

          A command to format the code using Prettier.

        - `"generate:component": "turbo gen react-component"`

          A shortcut command to generate a new React component using Turborepo’s built-in generator.

        - `"lint": "eslint . --max-warnings 0"`

          A shortcut command to run linting with strict settings.

        - `"clean": "git clean -xdf .cache .turbo node_modules"`

          A command to clean up caches and node modules for a fresh install.

        - `"check-types": "tsc --noEmit"`

          A shortcut command to run type checking with strict settings.
    
      - `"devDependencies"`
      
        The workspace dependencies (`workspace:*`) ensure that shared configurations for ESLint, Prettier, Tailwind, and TypeScript are used from the monorepo. Even if ESLint or TypeScript are installed in a shared config, including them here ensures that this package can run its commands independently.

      - `"prettier": "@craftzcode/prettier-config"`

        Specifies that the package should use the shared Prettier configuration for consistent code formatting.

  - Install shadcn-ui additional dependencies
    ```shell
    pnpm --filter @craftzcode/ui add class-variance-authority clsx tailwind-merge lucide-react
    ```

    **Explanations**
      - `pnpm --filter`

        When you run a command with `pnpm --filter <package>`, it targets a specific package within your monorepo. For example, `pnpm --filter @craftzcode/ui add ...` installs dependencies only in the `@craftzcode/ui` package.
  
  - Edit (or create) the `tsconfig.json` file inside `packages/ui` with the following content.
    ```json
    {
      "extends": "@craftzcode/typescript-config/    react-library.json",
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

    **Explanation**
      - `"extends": "@craftzcode/typescript-config/react-library.json"`

        Inherits TypeScript settings from a shared configuration tailored for React libraries.

      - `"baseUrl": "."`

        Specifies that the package should use the current directory as the base directory for resolving relative paths.

      - `"paths":`

        Configures path aliases for TypeScript imports.

      - `"@craftzcode/ui/*": ["./src/*"]`

        Maps the `@craftzcode/ui` package to the `src` directory within the package.

      - `"include": ["."]`

        Specifies that the package should include the current directory as part of its build.

      - `"exclude": ["node_modules", "dist"]`

        Excludes specific directories from the build.

      **Example Usage**

        ```js
        import { cn } from '@craftzcode/ui/lib/utils'
        ```

  - Inside `packages/ui/src/lib`, create a file named `utils.ts` with the following content.

    ```ts
    import type { ClassValue } from 'clsx'
    import { clsx } from 'clsx'
    import { twMerge } from 'tailwind-merge'

    export function cn(...inputs: ClassValue[]) {
      return twMerge(clsx(inputs))
    }
    ```

    **Explanation**

      This utility function, `cn`, combines the power of the `clsx` library (which conditionally joins class names) with `tailwind-merge` (which intelligently merges Tailwind CSS classes). This ensures that your component class names are both concise and conflict-free.

  - Inside the `packages/ui` folder, create a file named `components.json` with the following content.

    ```json
    {
      "$schema": "https://ui.shadcn.com/schema.   json",
      "style": "new-york",
      "rsc": true,
      "tsx": true,
      "tailwind": {
        "config": "",
        "css": "../../config/tailwind-config/   style.css",
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

  - Add the following scripts to your root `package.json`.

    ```json
    {
      "scripts": {
        "ui-add": "turbo run ui-add -F    @craftzcode/ui --",
        "clean": "git clean -xdf node_modules",
        "clean:workspaces": "turbo run clean"
      }
    }
    ```

    **Explanation**

      - `"ui-add": "turbo run ui-add -F @craftzcode/ui --"`

        This script runs the `ui-add` command (defined in the `@craftzcode/ui` package) using Turbo. The `-F @craftzcode/ui` flag filters the run to that specific package, and the `--` separator ensures any additional arguments are passed directly to the `ui-add` command.

      - `"clean:workspaces": "turbo run clean"`

        Executes the `clean` script across all workspaces via Turbo.

    - Add the following entries to your turbo.json.

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

      **Explanation**

        - `"ui-add"` in Turbo.

          Disables caching (`"cache": false`) to ensure the UI generator always runs interactively (using `"interactive": true`). This is useful because generating UI components often requires user input or prompts.

        - `"clean"` in Turbo.

          Disables caching for the `clean` command so that it always performs a fresh cleanup across all workspaces.

  - Git Commit

    ```shell
    git commit -m "feat(shadcn-ui): setup shared shadcn-ui package"
    ```

## Project Configuration

1. **Primary References**
   - [turborepo-shadcn-ui-tailwind-4](https://github.com/linkb15/turborepo-shadcn-ui-tailwind-4)
   - [rt-stack](https://github.com/nktnet1/rt-stack)
   - [Shadcn UI Documentation for Monorepo](https://ui.shadcn.com/docs/monorepo)
2. **Additional Templates**
   - [create-t3-turbo](https://github.com/t3-oss/create-t3-turbo) - Official template with tRPC, shadcn/ui, Tailwind CSS, and Expo
   - [turborepo-shadcn-ui](https://github.com/dan5py/turborepo-shadcn-ui) - Uses Tailwind CSS v3 and React v18
   - [turborepo-nextjs-tailwind-trpc](https://github.com/ayungavis/turborepo-nextjs-tailwind-trpc) - Includes tRPC setup with Next.js v14

### Configuration Notes

Initial ideas and configurations were influenced by this [Stack Overflow Discussion](https://stackoverflow.com/questions/79416157/how-to-enable-tailwindcss-v4-0-for-the-packages-ui-components-in-a-turborepo)
