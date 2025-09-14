# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Turborepo monorepo starter with TypeScript, Next.js 15, React 19, and shared packages. It includes two Next.js applications (`web` and `docs`) and shared packages for UI components, ESLint configuration, and TypeScript configuration.

## Architecture

### Monorepo Structure
- **apps/web** - Main Next.js application (port 3000)
- **apps/docs** - Documentation Next.js application (port 3001)
- **packages/ui** - Shared React component library (`@repo/ui`)
- **packages/eslint-config** - Shared ESLint configurations (`@repo/eslint-config`)
- **packages/typescript-config** - Shared TypeScript configurations (`@repo/typescript-config`)

### Key Technologies
- **Turborepo** - Monorepo build system with caching
- **Next.js 15** - React framework with App Router
- **React 19** - Latest React version
- **TypeScript 5.9** - Type safety
- **PNPM** - Package manager with workspaces
- **ESLint 9** - Code linting with zero warnings policy
- **Prettier** - Code formatting

### Package Architecture
- All packages use workspace protocol (`workspace:*`) for internal dependencies
- UI package exports components via path mapping (`./*": "./src/*.tsx"`)
- ESLint config provides base, Next.js, and React internal configurations
- TypeScript config provides base, Next.js, and React library configurations

## Development Commands

### Root Commands
```bash
# Development - runs all apps in parallel
pnpm dev
# or
turbo dev

# Build all packages and apps
pnpm build
# or
turbo build

# Lint all packages with zero warnings policy
pnpm lint
# or
turbo lint

# Type check all packages
pnpm check-types
# or
turbo check-types

# Format code
pnpm format
```

### App-Specific Commands
```bash
# Target specific app/package
turbo dev --filter=web
turbo build --filter=docs
turbo lint --filter=@repo/ui

# Individual app development
cd apps/web && pnpm dev      # Runs on port 3000
cd apps/docs && pnpm dev     # Runs on port 3001
```

### UI Package Commands
```bash
cd packages/ui

# Generate new React component
pnpm generate:component
# or
turbo gen react-component
```

## Key Files and Configurations

### Turbo Configuration (`turbo.json`)
- **build**: Depends on upstream builds, includes environment variables
- **lint**: Cascading lint with zero warnings
- **check-types**: TypeScript checking across packages
- **dev**: Non-cached persistent development mode

### Next.js Apps
- Both apps use **App Router** (not Pages Router)
- **Turbopack** enabled for development builds
- **Geist fonts** (Sans and Mono) configured as local fonts
- **Zero warnings policy** enforced in lint scripts

### Shared UI Package
- Components exported with path mapping
- Uses `"use client"` directive for client components
- Includes `appName` prop pattern for app-specific behavior

## Package Management

### Installation
- Uses **PNPM 9.0.0** with workspace support
- Node.js 18+ required
- Dependencies managed at workspace root and individual packages

### Workspace Dependencies
- Internal packages referenced with `workspace:*`
- External dependencies managed per package
- Shared dev dependencies (TypeScript, ESLint) at appropriate levels

## Development Workflow

### Adding New Components
1. Use the component generator: `turbo gen react-component` in `packages/ui`
2. Export from `packages/ui/src/[component].tsx`
3. Import in apps using `@repo/ui/[component]`

### Adding New Apps
1. Create new directory in `apps/`
2. Add to workspace in root `package.json`
3. Configure turbo tasks in `turbo.json`
4. Add shared package dependencies

### Linting and Type Safety
- **Zero warnings policy** enforced across all packages
- ESLint extends from shared configs (`@repo/eslint-config`)
- TypeScript strict mode with shared configurations
- Pre-commit hooks should run `lint` and `check-types`

## Turborepo Features

### Caching
- Build artifacts cached locally and optionally remotely
- Task dependencies ensure correct build order
- Incremental builds skip unchanged packages

### Filtering
- Target specific packages: `--filter=web`
- Target package groups: `--filter=@repo/*`
- Dependency-aware task execution

### Remote Caching (Optional)
```bash
# Setup remote caching with Vercel
turbo login
turbo link
```

## Notes for Development

- All packages are 100% TypeScript
- App Router is used (not Pages Router)
- Components should be client-side when interactivity is needed
- Follow the existing button component pattern for shared UI
- Maintain zero warnings policy for production readiness
- Use turbo commands at root, npm scripts within individual packages