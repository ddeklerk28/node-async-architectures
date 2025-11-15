# Node Async Architectures - Project Structure

## Overview
A monorepo containing reusable async architecture pattern libraries for Node.js, built with TypeScript. Libraries are published under the `@forge` scope and include implementations for message queues, pub/sub, event streaming, and more.

## Project Goals
- **Reusable**: Clean, documented libraries that can be used in any Node.js project
- **Type-safe**: Full TypeScript support with strict mode
- **Testable**: Comprehensive unit tests for each package
- **Practical**: Real-world examples with separate server/client implementations
- **Publishable**: Packages can be published to npm or used via GitHub

## Directory Structure

```
node-async-architectures/
├── packages/                           # Published library packages
│   ├── core/                          # @forge/core
│   │   ├── src/
│   │   │   ├── types/                # Shared types and interfaces
│   │   │   │   ├── index.ts
│   │   │   │   ├── job.types.ts
│   │   │   │   ├── processor.types.ts
│   │   │   │   └── events.types.ts
│   │   │   ├── base/                 # Base classes for patterns
│   │   │   │   ├── index.ts
│   │   │   │   ├── BaseQueue.ts
│   │   │   │   ├── BaseWorker.ts
│   │   │   │   └── BaseProcessor.ts
│   │   │   ├── utils/                # Shared utilities
│   │   │   │   ├── index.ts
│   │   │   │   ├── logger.ts
│   │   │   │   └── id-generator.ts
│   │   │   └── index.ts              # Main exports
│   │   ├── tests/                    # Unit tests
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── message-queue/                 # @forge/message-queue
│   │   ├── src/
│   │   │   ├── MessageQueue.ts
│   │   │   ├── MessageQueueWorker.ts
│   │   │   ├── JobManager.ts
│   │   │   ├── GroupManager.ts
│   │   │   ├── ProcessorRegistry.ts
│   │   │   └── index.ts
│   │   ├── tests/
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   ├── pub-sub/                       # @forge/pub-sub (future)
│   │   ├── src/
│   │   ├── tests/
│   │   ├── package.json
│   │   └── tsconfig.json
│   │
│   └── event-stream/                  # @forge/event-stream (future)
│       ├── src/
│       ├── tests/
│       ├── package.json
│       └── tsconfig.json
│
├── examples/                          # Example implementations (not published)
│   ├── message-queue/
│   │   ├── server/                   # Queue server with workers
│   │   │   ├── src/
│   │   │   │   ├── index.ts         # Server entry point
│   │   │   │   └── processors/      # Example job processors
│   │   │   │       ├── pdf-processor.ts
│   │   │   │       ├── email-processor.ts
│   │   │   │       └── image-processor.ts
│   │   │   ├── package.json
│   │   │   └── tsconfig.json
│   │   │
│   │   ├── client/                   # Job submitter
│   │   │   ├── src/
│   │   │   │   ├── index.ts         # Client entry point
│   │   │   │   └── scenarios/       # Different usage scenarios
│   │   │   │       ├── single-job.ts
│   │   │   │       ├── batch-jobs.ts
│   │   │   │       └── mixed-types.ts
│   │   │   ├── package.json
│   │   │   └── tsconfig.json
│   │   │
│   │   └── README.md                 # How to run the example
│   │
│   ├── pub-sub/                      # Future examples
│   │   ├── server/
│   │   ├── client/
│   │   └── README.md
│   │
│   └── shared/                       # Shared utilities across examples
│       ├── src/
│       │   ├── test-data.ts
│       │   └── helpers.ts
│       ├── package.json
│       └── tsconfig.json
│
├── docs/                             # Documentation
│   ├── getting-started.md
│   ├── architecture.md
│   ├── patterns/
│   │   ├── message-queue.md
│   │   ├── pub-sub.md
│   │   └── event-stream.md
│   └── api/                          # Auto-generated API docs
│
├── scripts/                          # Build and utility scripts
│   ├── build-all.sh
│   ├── clean.sh
│   └── publish.sh
│
├── .gitignore
├── .eslintrc.js
├── .prettierrc
├── package.json                      # Root workspace config
├── tsconfig.json                     # Base TypeScript config
├── tsconfig.build.json              # Build-specific config
├── vitest.config.ts                 # Shared Vitest config
├── README.md
└── PROJECT_STRUCTURE.md             # This file
```

## Technology Stack

### Core Technologies
- **Runtime**: Node.js 18+
- **Language**: TypeScript 5.3+
- **Package Manager**: npm with workspaces

### Build & Bundling
- **Compiler**: TypeScript with project references
- **Bundler**: esbuild (via tsup) for fast library builds
- **Watch Mode**: TypeScript watch for development

### Testing
- **Framework**: Vitest
- **Coverage**: Built-in Vitest coverage (c8)
- **Location**: Unit tests in each package's `tests/` folder

### Code Quality
- **Linting**: ESLint with TypeScript plugin
- **Formatting**: Prettier
- **Type Checking**: TypeScript strict mode

### Publishing
- **Scope**: `@forge/*`
- **Registry**: npm (future) / GitHub packages (initial)
- **Versioning**: Manual for now, potentially Changesets later

## Package Architecture

### Dependency Graph
```
@forge/core (no dependencies)
    ↑
    ├── @forge/message-queue
    ├── @forge/pub-sub
    └── @forge/event-stream
        ↑
        └── examples (dev dependencies only)
```

### Package Naming Convention
- **Core**: `@forge/core` - Types, interfaces, base classes
- **Patterns**: `@forge/[pattern-name]` - Individual implementations
- **Examples**: Not published, internal only

## Workspace Configuration

### Root package.json
```json
{
  "private": true,
  "workspaces": [
    "packages/*",
    "examples/*/server",
    "examples/*/client",
    "examples/shared"
  ]
}
```

### Benefits
- Single `npm install` installs all dependencies
- Shared `node_modules` at root
- Easy cross-package linking during development
- Unified script execution

## Development Workflow

### Initial Setup
```bash
npm install                    # Install all dependencies
npm run build                  # Build all packages
```

### Development
```bash
npm run dev:core              # Watch mode for core
npm run dev:mq                # Watch mode for message-queue
npm run dev:mq:server         # Run message queue server example
npm run dev:mq:client         # Run message queue client example
```

### Testing
```bash
npm test                      # Run all tests
npm run test:core             # Test core only
npm run test:mq               # Test message-queue only
npm run test:watch            # Watch mode
npm run test:coverage         # Coverage report
```

### Code Quality
```bash
npm run lint                  # Lint all packages
npm run format                # Format all files
npm run typecheck             # Type check all packages
```

### Building
```bash
npm run build                 # Build all packages
npm run build:core            # Build core only
npm run build:mq              # Build message-queue only
npm run clean                 # Clean all build outputs
```

## Example Usage Pattern

### Running Examples
The examples demonstrate real-world usage with separate server/client:

```bash
# Terminal 1: Start the server (workers)
npm run dev:mq:server

# Terminal 2: Run the client (submit jobs)
npm run dev:mq:client
```

### Example Structure Benefits
1. **Realistic**: Mimics production deployment
2. **Isolated**: Server and client are independent
3. **Flexible**: Can run multiple clients or servers
4. **Educational**: Clear separation shows responsibilities

## Publishing Strategy

### Phase 1: Internal (Current)
- Use via GitHub with npm workspaces
- Install from GitHub: `npm install github:user/repo#workspace=@forge/core`

### Phase 2: GitHub Packages
- Publish to GitHub Package Registry
- Scoped to organization

### Phase 3: npm Registry (Future)
- Public npm packages
- Automated releases via CI/CD
- Semantic versioning

## Build Strategy

### Development
1. TypeScript compiles in watch mode
2. Examples use `tsx` or `ts-node` for direct execution
3. Fast feedback loop

### Production
1. Build core first (other packages depend on it)
2. Use tsup with esbuild for fast bundling
3. Generate `.d.ts` declaration files
4. Create distributable packages in `dist/`

### TypeScript Project References
- Enables incremental builds
- Proper dependency ordering
- Better IDE performance

## Next Steps

### Phase 1: Foundation (Current)
- [x] Define project structure
- [ ] Set up workspace configuration
- [ ] Configure TypeScript, ESLint, Prettier
- [ ] Create core package scaffolding
- [ ] Set up Vitest

### Phase 2: Core Package
- [ ] Define core types and interfaces
- [ ] Implement base classes
- [ ] Add utilities (logger, ID generator)
- [ ] Write unit tests
- [ ] Generate API documentation

### Phase 3: Message Queue
- [ ] Implement message queue based on existing design
- [ ] Create server example with processors
- [ ] Create client example with job submission
- [ ] Write comprehensive tests
- [ ] Document usage patterns

### Phase 4: Additional Patterns
- [ ] Implement pub/sub pattern
- [ ] Implement event stream pattern
- [ ] Add more examples
- [ ] Performance benchmarks

### Phase 5: Polish & Publish
- [ ] Complete documentation
- [ ] Set up GitHub Actions CI/CD
- [ ] Publish to GitHub Packages
- [ ] Consider npm publication

## Design Principles

1. **Separation of Concerns**: Core abstractions separate from implementations
2. **Type Safety**: Leverage TypeScript's type system fully
3. **Extensibility**: Easy to add new patterns without modifying core
4. **Testability**: Each package independently testable
5. **Developer Experience**: Clear examples, good documentation, fast builds
6. **Production Ready**: Patterns should work in real applications

## Questions & Decisions Log

### Resolved
- ✅ Monorepo tool: npm workspaces
- ✅ Build tool: esbuild (via tsup)
- ✅ Testing: Vitest
- ✅ Package scope: `@forge`
- ✅ Example structure: Separate server/client
- ✅ Unit tests: In each package's tests folder

### To Decide Later
- Version management strategy (manual vs Changesets)
- CI/CD platform (GitHub Actions)
- Documentation generator (TypeDoc vs custom)
- Performance benchmarking approach

