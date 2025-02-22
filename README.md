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

2. **Update Dependencies**

```shell
pnpm up --recursive
```

- Git Commit

```
git commit -m "chore(deps): bump dependencies across workspace"
```

3. **Set Up GitHub Repository**

- Create a new repository named `create-craftzcode` on GitHub
- Initialize and push your local repository:

```shell
git remote add origin git@github.com:craftzcode/create-craftzcode.git
git branch -M main
git push -u origin main
```

4. **Rename Workspace & Configure TypeScript & ESLint**

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

  - Rename all instances of `@craftzcode` to `@craftzcode` in the project. This ensures that imports will look like `@craftzcode/ui`.
  - Git Commit

    ```
    git commit -m "refactor: rename package namespace from "@craftzcode" to "@craftzcode""
    ```

- **Configure TypeScript & ESLint**
  - Create a new folder named `config` in the root of your project.
  - Move the `typescript-config` and `eslint-config` into the `config` folder.
  - Add the `config` folder to `pnpm-workspace.yaml`.
  - Reinstall packages:
    ```shell
    pnpm install
    ```
  - Git Commit
    ```shell
    git commit -m  "chore(config): setup config folder for TypeScript & ESLint"
    ```

## Project Configuration

### Reference Templates

We'll be following these templates to set up Turborepo with shadcn/ui and Tailwind CSS v4:

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
