Setup CratzCode Stack

Tech Stack:
Next.js
tRPC
Tanstack Query
Drizzle ORM
Postgres
Better-Auth
Tailwind CSS
Shadcn
Expo

Stack Components
Below is an overview of the project structure and its key components:

```
apps
  ├─ docs
  |   ├─ Next.js 15
  |   ├─ React 19
  |   ├─ Tailwind CSS
  |   └─ E2E Typesafe API Server & Client
  └─ web
  |   ├─ Next.js 15
  |   ├─ React 19
  |   ├─ Tailwind CSS
  |   └─ E2E Typesafe API Server & Client
  ├─ mobile
  |   ├─ Expo SDK 51
  |   ├─ React Native using React 18
  |   ├─ Navigation using Expo Router
  |   ├─ Tailwind using NativeWind
  |   └─ Typesafe API calls using tRPC
packages
  ├─ api
  |   └─ tRPC v11 router definition
  ├─ auth
  |   └─ Better-Auth
  ├─ db
  |   └─ Typesafe db calls using Drizzle & Neon Tech
  └─ ui
  |   └─ UI package for the webapp using shadcn-ui
config
  ├─ eslint-config
  │    └─ Shared, fine-grained ESLint presets
  ├─ prettier-config
  │    └─ Shared Prettier configuration
  ├─ tailwind-config
  │    └─ Shared Tailwind CSS configuration
  └─ typescript-config
       └─ Shared TSConfig to extend from
```

1.  Create Turborepo Project

    - CLI: `pnpm dlx create-turbo@latest craftzcode-stack`
    - Package Manager: `Bun`
    - Update Dependencies in all Workspace: `bun update`
  
3. Add Husky (Pre-Push) for Automation of Git Push

   - CLI: `bun add --dev husky`
   - Init Husky: `bunx husky init`
   - Delete the `pre-commit` file inside of `.husky` folder and create a new one named `pre-push` with the following content.
     ```sh
     #!/bin/sh
      . "$(dirname "$0")/_/husky.sh"

      # Get the current branch name
      current_branch=$(git rev-parse --abbrev-ref HEAD)

      if [ "$current_branch" = "main" ]; then
        echo "Warning: You are pushing from the 'main' branch."
        read -p "Would you like to create a new branch for these changes? (y/n): " answer
      
        if [ "$answer" = "y" ] || [ "$answer" = "Y" ]; then
          # Prompt for a new branch name; generate one if none is provided.
          read -p "Enter the new branch name (or leave empty for a generated name): " new_branch
          if [ -z "$new_branch" ]; then
            new_branch="feature-$(date +%Y%m%d%H%M%S)"
            echo "No branch name provided. Using generated branch name: $new_branch"
          fi
          # Create and switch to the new branch
          git checkout -b "$new_branch"
          echo "Switched to new branch '$new_branch'. Please push your changes from this branch."
          exit 1  # Abort the push so you can push from the new branch
        else
          echo "Proceeding with push on 'main'."
        fi
      fi

      exit 0
     ```

2.  Remove Boilerplates

    - Delete `page.module.css` and all of code inside of `page.tsx` both on `docs and web`.
    - GIT COMMIT: `git commit -m "refactor(docs|web): delete boilerplates"`

3.  Create a Github Repository

    - Add the github repository to the project and push the main branch.

4.  Add Cursor Rules, Rename Workspace, Configure Typescript & Eslint

    - Add Cursor Rules

      - Copy all of my cursor rules in `.cursor/rules/{rules}.mdc`.
      - GIT COMMIT: `feat(cursor): add custom rules for Cursor AI Editor`

    - Rename Package Imports

      - Rename all instances of `@repo` to `@craftzcode` in the project. This ensures that imports will look like `@craftzcode/ui`.
      - GIT COMMIT: `git commit -m "refactor(alias): rename @repo to @craftzcode"`

    - Configure TypeScript & ESLint

      - Create a new folder named `config` in the root of your project.
      - Move the `typescript-config` and `eslint-config` into the `config` folder.
      - Add the `config` folder to `workspaces` in the `package.json` of the root of your project.
      - Reinstall packages.
        - CLI: `bun install`
      - GIT COMMIT: `git commit -m "chore(config): move typescript and eslint configs to config folder"`

    - GIT BRANCH: `git checkout -b infrastructure/chore/1-cursor-alias-config`
    - PULL REQUEST TITLE: `chore(infrastructure): add cursor rules, update aliases, and reorganize configs`

5.  Setup Prettier, Tailwind CSS, and Shadcn UI Shared Configs

    ```
    craftzcode-stack/
    ├── config/
    │ ├── prettier-config/
    │ │ ├── index.js
    │ │ └── package.json
    │ └── tailwind-config/
    │ ├── package.json
    │ ├── postcss.config.mjs
    │ └── style.css
    ├── packages/
    │ └── ui/ (shared shadcn-ui package)
    │ ├── package.json
    │ ├── tsconfig.json
    │ └── src/
    │ ├── lib/
    │ │ └── utils.ts
    │ ├── components/ (all existing components here should be deleted)
    │ └── hooks/ (if any)
    ├── turbo.json (with additional commands: ui-add & clean)
    └── package.json (root, with shared scripts)
    ```

    - Create the Prettier Config Package

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
            "@craftzcode/eslint-config": "*"
          },
          "prettier": "@craftzcode/prettier-config"
        }
        ```

      - Go to the location of your `prettier-config` folder in shell then install `prettier` and `plugins`

        ```shell
        bun add -D prettier @ianvs/prettier-plugin-sort-imports prettier-plugin-tailwindcss
        ```

      - Inside the `config/prettier-config` folder, create an `index.js` file with the following content.

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

      - GIT COMMIT: `git commit -m "chore(prettier): add shared prettier config for all workspaces"`

    - Create the Tailwind CSS Config Package

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
            "@craftzcode/eslint-config": "*",
            "@craftzcode/prettier-config": "*",
            "eslint": "^9.22.0"
          },
          "prettier": "@craftzcode/prettier-config"
        }
        ```

      - Go to the location of your `tailwind-config` folder in shell then install `tailwind`

        ```shell
        bun add -D tailwindcss @tailwindcss/postcss
        bun add tw-animate-css
        ```

      - Inside the `config/tailwind-config` folder, create a file named `postcss.config.mjs` with the following content.

        ```js
        const config = {
          plugins: ["@tailwindcss/postcss"],
        };

        export default config;
        ```

      - Inside the same `config/tailwind-config` folder, create a file named `style.css` and copy and paste the content of `globals.css` in the existing next.js project with a latest shadcn and add also this `@source '../../packages/ui';` to include the shadcn shared config in `packages/ui`.
        
      - GIT COMMIT: `git commit -m "chore(tailwind): add shared tailwind css config for all workspaces"`

    - Setup shared shadcn-ui

      - Delete all files inside the `packages/ui/src/components` folder to prepare for shadcn-ui’s generated components.
      - Replace the contents of `packages/ui/package.json` with the following code.

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

      - Go to the location of your `packages/ui` folder in shell then install shadcn-ui additional dependencies

        ```shell
        bun add class-variance-authority clsx tailwind-merge lucide-react
        ```

      - Edit (or create) the `tsconfig.json` file inside `packages/ui` with the following content.

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

      - Inside `packages/ui/src/lib`, create a file named `utils.ts` with the following content.

        ```ts
        import { clsx, type ClassValue } from "clsx";
        import { twMerge } from "tailwind-merge";

        export function cn(...inputs: ClassValue[]) {
          return twMerge(clsx(inputs));
        }
        ```

      - Inside the `packages/ui` folder, create a file named `components.json` with the following content.

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

      - Add the following scripts to your root `package.json`.

        ```json
        {
          "scripts": {
            "ui-add": "turbo run ui-add -F @craftzcode/ui --",
            "clean": "git clean -xdf node_modules",
            "clean:workspaces": "turbo run clean"
          }
        }
        ```

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
      - GIT COMMIT: `git commit -m "chore(shadcn): remove original files in packages/ui and setup shadcn"`

    - GIT BRANCH: `git checkout -b infrastructure/chore/2-shared-config`
    - PULL REQUEST TITLE: `chore(infrastructure): add shared prettier & tailwind configs and setup shadcn in packages/ui`

    - How to use all shared config
      - Prettier Shared Config
        1. Always add `"@rhu-ii/prettier-config": "*"` in the `devDependencies` of `package.json` where workspace are you currently working on it.
        2. Always add `"prettier": "@rhu-ii/prettier-config"` in the `package.json` where workspace are you currently working on it.
      - Tailwind CSS Shared Config
        1. Always add `"@rhu-ii/tailwind-config": "*"` in the `devDependencies` of `package.json` where web apps are you currently working on it.
        2. Always add `@import "@rhu-ii/tailwind-config/style.css";` in the `globals.css` where web apps are you currently working on it.
      - Typescript and Eslint Shared Config
        - Always add `"@rhu-ii/typescript-config": "*"` and `"@rhu-ii/eslint-config": "*"` where workspace are you currently working on it.
