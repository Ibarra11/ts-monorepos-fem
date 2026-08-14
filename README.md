# TypeScript Monorepo Learning Project

A hands-on TypeScript monorepo project completed as part of a **Frontend Masters TypeScript Monorepos course**.

The project starts as a small client/server seed catalog application built with Svelte, TypeScript, and Express and is progressively reorganized into a monorepo. I used the project to learn how multi-package repositories manage shared code, dependencies, builds, testing, TypeScript configuration, task orchestration, caching, and CI.

## What I Learned

Through this project, I worked with the core concepts involved in building and maintaining a production-style TypeScript monorepo.

### Monorepo Architecture

- Structuring multiple packages inside a single repository
- Breaking a monolithic application into independent workspace packages
- Starting package extraction from the lowest level of the dependency graph
- Managing dependencies between packages
- Creating clear public APIs between packages
- Using scoped package names such as `@seeds/models`
- Using barrel exports to avoid importing package internals

### pnpm Workspaces

- Configuring a workspace with `pnpm-workspace.yaml`
- Linking local packages with the `workspace:` protocol
- Understanding `workspace:*`
- Running scripts recursively across workspace packages
- Filtering commands to individual packages
- Understanding how pnpm represents workspace packages in `pnpm-lock.yaml`
- Preserving locked dependency versions while restructuring a repository

Example workspace dependency:

```json
{
	"dependencies": {
		"@seeds/models": "workspace:*"
	}
}
```

### TypeScript in a Monorepo

- Sharing TypeScript configuration across packages
- Extending a root `tsconfig.json`
- Understanding how `include` behaves when extending configurations
- Separating development configuration from production compilation with `tsconfig.build.json`
- Generating declaration files and declaration maps
- Using TypeScript project references
- Using `composite: true` and `tsc -b` for dependency-aware builds
- Improving TypeScript performance as a monorepo grows

Example project relationship:

```text
       models
       ↗    ↖
     ui    server
```

Both the UI and server can depend on the shared models package without depending on each other.

### Dependency & Repository Consistency

I also explored tooling for keeping a larger monorepo maintainable:

- **Manypkg** — validates `package.json` files and package conventions
- **Syncpack** — detects inconsistent dependency versions between packages
- **Knip** — detects potentially unused dependencies, files, and exports
- **Volta** — keeps Node.js and package-manager versions consistent between development environments

### Task Orchestration with Lerna & Nx

- Running tasks across multiple packages with Lerna
- Filtering tasks to specific packages
- Running tasks only for packages affected by a Git change
- Understanding project and task dependency graphs
- Configuring task dependencies with Nx
- Caching deterministic tasks such as builds, tests, and linting
- Understanding how caching can reduce repeated work locally and in CI

For example:

```bash
pnpm lerna run build,lint,test,check --scope=@seeds/ui
```

Affected packages can also be determined relative to a Git ref:

```bash
pnpm lerna run test --since main
```

This allows CI to avoid rebuilding and testing unrelated packages.

### API Management & Documentation

- Using **API Extractor** to define and validate package API surfaces
- Generating `.d.ts` rollups
- Using release tags such as `@public`, `@beta`, `@alpha`, and `@internal`
- Detecting accidental public API changes through API reports
- Using **API Documenter** to generate Markdown documentation from package APIs

### Git & CI Practices

- Preserving Git history while moving files into packages
- Separating structural moves from behavioral changes
- Using `CODEOWNERS` to establish ownership of different monorepo packages
- Running CI tasks based on affected packages instead of the entire repository
- Using caching to reduce repeated CI work
- Managing package versions and changelogs with Changesets

## Tech Stack

| Area               | Technology                          |
| ------------------ | ----------------------------------- |
| Frontend           | Svelte 5, TypeScript                |
| Backend            | Express.js, TypeScript              |
| Package Manager    | pnpm                                |
| Monorepo Tooling   | Lerna, Nx                           |
| Build Tool         | Vite                                |
| Testing            | Vitest, Testing Library             |
| Linting            | ESLint                              |
| Styling            | Tailwind CSS, DaisyUI, Sass         |
| API Tooling        | API Extractor, API Documenter       |
| Repository Tooling | Manypkg, Syncpack, Knip, Changesets |
| Runtime Management | Volta                               |

## Development

### Prerequisites

- Node.js 22.16.0+
- pnpm 10+
- Git

Install dependencies:

```bash
pnpm install
```

Start the development environment:

```bash
pnpm run dev
```

Run the primary project checks:

```bash
pnpm run build
pnpm run lint
pnpm run test
pnpm run check
```

## Useful Commands

```bash
# Development
pnpm run dev

# Build packages
pnpm run build

# Type checking
pnpm run check

# Linting
pnpm run lint

# Tests
pnpm run test

# Watch tests
pnpm run test:watch

# Test coverage
pnpm run test:coverage
```

## Core Takeaways

The biggest lesson from this project was that a monorepo is more than simply putting multiple projects into the same Git repository.

A well-structured monorepo provides:

```text
Shared repository
      ↓
Clear package boundaries
      ↓
Explicit dependency graph
      ↓
Dependency-aware task execution
      ↓
Caching + affected CI
      ↓
Faster and safer development
```

The tooling around the repository is what makes the architecture scalable. pnpm manages workspace packages and dependencies, TypeScript project references define relationships between TypeScript projects, and Lerna/Nx use the dependency graph to efficiently orchestrate builds, tests, linting, and other tasks.

The project also reinforced the importance of maintaining clear public APIs between packages. Even though monorepos make sharing code easy, packages should still have intentional boundaries so that the repository does not gradually become another tightly coupled monolith.

## Course

This project was completed while working through the **Frontend Masters TypeScript Monorepos course** and is intended as a hands-on learning project rather than an original production application.

The repository contains my implementation and exercises used to practice modern TypeScript monorepo architecture and tooling.
