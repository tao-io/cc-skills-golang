---
name: golang
description: "Idiomatic Go engineering — CLI tooling (spf13/cobra, spf13/viper, urfave/cli), project layout, naming, error handling (errors.Is/As, %w, samber/oops), context, concurrency, testing (testify, table-driven), benchmarking & pprof, dependency injection (samber/do, uber/fx, uber/dig, google/wire), observability (slog, OpenTelemetry, Prometheus, Pyroscope), performance optimisation, security & safety, database access (database/sql, sqlx, pgx, GORM), gRPC/GraphQL/Swagger, samber utility libraries (lo, mo, ro, hot, oops, slog-*), modernisation, troubleshooting. Apply to any Go work — *.go files, go.mod, or imports of the listed libraries. Loads detailed sub-skills on demand via Read tool; never preload everything."
user-invocable: true
license: MIT
allowed-tools: Read Edit Write Glob Grep Bash(go:*) Bash(golangci-lint:*) Bash(git:*) Agent WebFetch
---

# golang — meta-skill dispatcher

> **Origin.** This skill is a tao-io maintained fork of
> [samber/cc-skills-golang](https://github.com/samber/cc-skills-golang)
> (MIT, by [@samber](https://github.com/samber)). Every sub-skill in this
> directory is a 1-to-1 copy of an upstream skill with two structural
> changes: the `golang-` prefix is dropped from directory names, and the
> inner skill manifest is renamed `SKILL.md` → `sub-SKILL.md` so that
> Claude Code's auto-discovery never surfaces sub-skills as standalone
> entries in the chooser. The upstream `references/` and `evals/`
> subdirectories are preserved verbatim inside each sub-skill.
>
> Why: 43 individual Go skills would crowd the global chooser and dilute
> selection. This meta-skill is the single discoverable entry; it loads
> sub-skills on demand via the `Read` tool when a task matches one of
> their triggers.
>
> Sync from upstream:
> ```bash
> git fetch upstream && git cherry-pick <commit>   # selective
> ```

## How to use this skill

You are working on a Go task. **Do not read every sub-skill up front.**
Instead:

1. Identify which sub-skill(s) the current task matches by scanning the
   **Dispatch** section below — focus on triggers (imports, file globs,
   problem phrasing).
2. Read the matching `<topic>/sub-SKILL.md` via the `Read` tool.
3. If that sub-skill references further detail in its `references/`
   directory, read those files too — they hold the deep specifics.
4. For multi-topic tasks (e.g. "build a Cobra CLI with Viper config and
   testify tests"), load several sub-skills in parallel.
5. The `how-to/sub-SKILL.md` sub-skill is an orchestrator for ambiguous
   multi-cluster tasks — read it first when uncertain which sub-skills
   apply.

Each sub-skill also contains an `evals/evals.json` with prompt/trap/
assertions test cases — useful as a verification spec when you write
new Go code that the sub-skill governs.

## Always-on minimum

Apply regardless of which sub-skill is active:

- `gofmt` / `goimports` on every file before considering work complete.
- Error wrapping: `fmt.Errorf("doing X: %w", err)` — never `errors.New`
  for wrapping; never bare `return err` if context can be added.
- `errors.Is` / `errors.As` for inspection — never string-match.
- `panic` only inside `main()` / `init()` or a clearly documented
  programmer-error path. Library code returns errors.
- `any` instead of `interface{}` (Go 1.18+).
- Receiver names are 1-2 letters reflecting the type (`c *Client`, not
  `this` or `self`).
- No `init()` for application setup — wire explicitly in `main()`.
- Tests live next to code (`foo_test.go`), use table-driven form, and
  call `t.Parallel()` where safe.

For details on any of the above, dispatch to the relevant sub-skill.

## Dispatch

### CLI & configuration

- **`spf13-cobra/sub-SKILL.md`** — Golang CLI command tree library using spf13/cobra — cobra.Command, RunE vs Run, PersistentPreRunE hook chain, Args validators (NoArgs, ExactArgs, MatchAll, custom), persistent vs local flags, command groups, ValidArgsFunction, RegisterFlagCompletionFunc, ShellCompDirective, usage/help template customization, man-page and markdown doc generation, and testing with SetArgs/SetOut/SetErr. Apply when using or adopting spf13/cobra, or when the codebase imports `github.com/spf13/cobra`. For configuration layering alongside cobra, see the `spf13-viper/sub-SKILL.md` skill. For general CLI architecture (project layout, exit codes, signal handling, I/O patterns), see `cli/sub-SKILL.md`.

- **`spf13-viper/sub-SKILL.md`** — Golang configuration library using spf13/viper — layered precedence (flag > env > file > KV > default), BindPFlag/BindPFlags, SetEnvPrefix + SetEnvKeyReplacer + AutomaticEnv, ReadInConfig + ConfigFileNotFoundError, Unmarshal + mapstructure struct tags, Sub for sub-trees, WatchConfig + OnConfigChange for hot reload, viper.New() for test isolation, and remote KV integration. Apply when using or adopting spf13/viper, or when the codebase imports `github.com/spf13/viper`. For CLI command structure alongside viper, see the `spf13-cobra/sub-SKILL.md` skill. For general CLI architecture, see `cli/sub-SKILL.md`.

- **`cli/sub-SKILL.md`** — Golang CLI application development. Use when building, modifying, or reviewing a Go CLI tool — especially for command structure, flag handling, configuration layering, version embedding, exit codes, I/O patterns, signal handling, shell completion, argument validation, and CLI unit testing. Also triggers when code uses cobra, viper, or urfave/cli. For cobra-specific APIs → See `spf13-cobra/sub-SKILL.md` skill; for viper configuration layering → See `spf13-viper/sub-SKILL.md` skill.

### Project hygiene

- **`project-layout/sub-SKILL.md`** — Provides a guide for setting up Golang project layouts and workspaces. Use when starting a new Go project, organizing an existing codebase, setting up a monorepo with multiple packages, creating CLI tools with multiple main packages, deciding between cmd/internal/pkg directory conventions, or discussing package restructuring, package splits, or module splits.

- **`naming/sub-SKILL.md`** — Go (Golang) naming conventions — covers packages, constructors, structs, interfaces, constants, enums, errors, booleans, receivers, getters/setters, functional options, acronyms, test functions, and subtest names. Use this skill when writing new Go code, reviewing or refactoring, choosing between naming alternatives (New vs NewTypeName, isConnected vs connected, ErrNotFound vs NotFoundError, StatusReady vs StatusUnknown at iota 0), debating Go package names (utils/helpers anti-patterns), or asking about Go naming best practices. Also trigger when the user mentions MixedCaps vs snake_case, ALL_CAPS constants, Get-prefix on getters, or error string casing. Do NOT use for general Go implementation questions that don't involve naming decisions.

- **`code-style/sub-SKILL.md`** — Golang code style conventions — line length and breaking, variable declarations, control flow clarity, when comments help vs hurt. Use when writing or reviewing Go code, asking about style or clarity, or establishing project coding standards. Not for naming conventions (→ See `naming/sub-SKILL.md` skill), linter configuration (→ See `lint/sub-SKILL.md` skill), or doc comments (→ See `documentation/sub-SKILL.md` skill).

- **`lint/sub-SKILL.md`** — Linting best practices and golangci-lint configuration for Golang projects — running linters, configuring .golangci.yml, suppressing warnings with nolint directives, interpreting lint output, and selecting linters. Use when configuring golangci-lint, asking about lint warnings or nolint suppressions, setting up code quality tooling, or choosing linters. Also use when the user mentions golangci-lint, go vet, staticcheck, or revive.

- **`documentation/sub-SKILL.md`** — Comprehensive documentation guide for Golang projects, covering godoc comments, README, CONTRIBUTING, CHANGELOG, Go Playground, Example tests, API docs, and llms.txt. Use when writing or reviewing doc comments, documentation, adding code examples, setting up doc sites, or discussing documentation best practices. Triggers for both libraries and applications/CLIs.

- **`dependency-management/sub-SKILL.md`** — Dependency management strategies for Golang projects — go.mod management, installing/upgrading packages, Minimal Version Selection, vulnerability scanning, outdated dependency tracking, binary size analysis, Dependabot/Renovate setup, conflict resolution, and go.work workspaces. Use when adding, removing, or upgrading Go dependencies, auditing vulnerabilities, resolving version conflicts, or setting up automated dependency updates.

- **`continuous-integration/sub-SKILL.md`** — CI/CD pipeline configuration using GitHub Actions for Golang projects — testing, linting, SAST, security scanning, code coverage, Dependabot, Renovate, GoReleaser, code review automation, and release pipelines. Use when setting up or improving Go project CI, configuring GitHub Actions workflows, adding linters or security scanners, automating dependency updates, or adding quality gates.

### Errors, concurrency, types

- **`error-handling/sub-SKILL.md`** — Idiomatic Golang error handling — creation, wrapping with %w, errors.Is/As, errors.Join, custom error types, sentinel errors, panic/recover, the single handling rule, structured logging with slog, HTTP request logging middleware, and samber/oops for production errors. Built to make logs usable at scale with log aggregation 3rd-party tools. Apply when creating, wrapping, inspecting, or logging errors in Go code. For samber/oops specifics → See `samber-oops/sub-SKILL.md` skill; for slog handler ecosystem → See `samber-slog/sub-SKILL.md` skill.

- **`context/sub-SKILL.md`** — Idiomatic use of context.Context in Golang — cancellation, deadlines, timeouts, propagation through call chains, context values (and when not to use them), goroutine lifecycle, and proper integration with database, HTTP, gRPC, and channel operations. Use when writing or reviewing code that accepts or creates a context.Context, when handling cancellation/timeout, when propagating request-scoped values, or when goroutines need lifecycle management.

- **`concurrency/sub-SKILL.md`** — Golang concurrency primitives and patterns — goroutines, channels (buffered, unbuffered, directional), select statements, sync package (Mutex, RWMutex, Once, WaitGroup, Cond, Pool, Map), atomic package, errgroup, context cancellation, worker pools, fan-in/fan-out, pipelines, semaphores, and the race detector. Apply when writing or reviewing concurrent Go code, choosing between mutex/channel/atomic, debugging races or deadlocks, or designing goroutine lifecycles. Not for general goroutine error handling (→ See `error-handling/sub-SKILL.md` skill) or context propagation (→ See `context/sub-SKILL.md` skill).

- **`structs-interfaces/sub-SKILL.md`** — Golang struct and interface design — struct field layout, tags, embedding, methods, value vs pointer receivers, interface design (small interfaces, accept-interface-return-struct, interface segregation), interface composition, type assertions, type switches, and zero-value usability. Apply when designing or refactoring Go types, choosing between struct embedding and composition, deciding method receiver kind, or auditing interface boundaries.

- **`data-structures/sub-SKILL.md`** — Golang data structures — slices (capacity, growth, aliasing), maps (iteration order, deletion during iteration, memory), arrays vs slices, generics with type parameters and constraints, common patterns (sets via map[T]struct{}, ordered maps, LRU sketches), and stdlib container packages (container/list, container/heap, container/ring). Apply when choosing or operating on Go collections, optimizing memory layout, or implementing generic helpers.

- **`design-patterns/sub-SKILL.md`** — Idiomatic Go design patterns — functional options, builder, factory, strategy, observer, visitor, middleware/decorator, adapter, and Go-specific variants. Apply when structuring a new package's public API, refactoring towards extensibility, or choosing between alternative designs (functional options vs config struct, interface segregation vs single big interface).

### Testing & benchmarking

- **`testing/sub-SKILL.md`** — Idiomatic Golang testing — table-driven tests, subtests with t.Run, t.Parallel, testdata directory, golden files, fuzzing, examples, helpers with t.Helper, test fixtures, httptest, sqlmock, and CI integration. Apply when writing or reviewing `*_test.go` files, organising test suites, debugging flaky tests, or designing test fixtures. For testify-specific APIs (require/assert/mock/suite) → See `stretchr-testify/sub-SKILL.md` skill. For benchmarking → See `benchmark/sub-SKILL.md` skill.

- **`stretchr-testify/sub-SKILL.md`** — Golang testing with stretchr/testify — assert vs require, suite organisation, mock generation and behaviour, helper APIs, custom assertions, and integration with table-driven tests. Apply when the codebase imports github.com/stretchr/testify, or when introducing a testify mock/suite into an existing testing setup.

- **`benchmark/sub-SKILL.md`** — Golang benchmarking, profiling, and performance measurement. Use when writing, running, or comparing Go benchmarks, profiling hot paths with pprof, interpreting CPU/memory/trace profiles, analyzing results with benchstat, setting up CI benchmark regression detection, or investigating production performance with Prometheus runtime metrics. Also use when the developer needs deep analysis on a specific performance indicator - this skill provides the measurement methodology, while `performance/sub-SKILL.md` provides the optimization patterns.

### Dependency injection

- **`dependency-injection/sub-SKILL.md`** — Comprehensive guide for dependency injection (DI) in Golang. Covers why DI matters (testability, loose coupling, separation of concerns, lifecycle management), manual constructor injection, and DI library comparison (google/wire, uber-go/dig, uber-go/fx, samber/do). Use this skill when designing service architecture, setting up dependency injection, refactoring tightly coupled code, managing singletons or service factories, or when the user asks about inversion of control, service containers, or wiring dependencies in Go. For a specific DI library, → See `google-wire/sub-SKILL.md`, `uber-dig/sub-SKILL.md`, `uber-fx/sub-SKILL.md`, or `samber-do/sub-SKILL.md` skills.

- **`samber-do/sub-SKILL.md`** — Dependency injection in Golang using samber/do — service containers, lifecycle management, scopes, health checks, graceful shutdown, and module organization. Apply when using or adopting samber/do, when the codebase imports github.com/samber/do or github.com/samber/do/v2, or when refactoring manual constructor injection into a DI container.

- **`uber-fx/sub-SKILL.md`** — Golang application framework using uber-go/fx — fx.New, fx.Provide, fx.Invoke, fx.Module, fx.Lifecycle hooks, fx.Annotate (name/group/As), fx.Decorate, fx.Supply, fx.Replace, fx.WithLogger, and signal-aware Run(). Apply when using or adopting uber-go/fx, when the codebase imports `go.uber.org/fx`, or when wiring services with fx.New. For raw DI without lifecycle, see `uber-dig/sub-SKILL.md` skill.

- **`uber-dig/sub-SKILL.md`** — Implements dependency injection in Golang using uber-go/dig — reflection-based container, Provide/Invoke, dig.In/dig.Out parameter and result objects, named values, value groups, optional dependencies, scopes, and Decorate. Apply when using or adopting uber-go/dig, when the codebase imports `go.uber.org/dig`, or when wiring an application graph at startup. For higher-level lifecycle and modules, see `uber-fx/sub-SKILL.md` skill.

- **`google-wire/sub-SKILL.md`** — Compile-time dependency injection in Golang using google/wire — wire.NewSet, wire.Build, wire.Bind (interface→concrete), wire.Struct, wire.Value, wire.InterfaceValue, wire.FieldsOf, cleanup functions, //go:build wireinject injector files, and generated wire_gen.go. Apply when using or adopting google/wire, when the codebase imports `github.com/google/wire`, or when wiring an application graph at compile time via `wire.Build`. For runtime DI with reflection, see `uber-dig/sub-SKILL.md` skill.

### Samber utility libraries

- **`samber-lo/sub-SKILL.md`** — Functional programming helpers for Golang using samber/lo — 500+ type-safe generic functions for slices, maps, channels, strings, math, tuples, and concurrency (Map, Filter, Reduce, GroupBy, Chunk, Flatten, Find, Uniq, etc.). Core immutable package (lo), concurrent variants (lo/parallel aka lop), in-place mutations (lo/mutable aka lom), lazy iterators (lo/it aka loi for Go 1.23+), and experimental SIMD (lo/exp/simd). Apply when using or adopting samber/lo, when the codebase imports github.com/samber/lo, or when implementing functional-style data transformations in Go. Not for streaming pipelines (→ See `samber-ro/sub-SKILL.md` skill).

- **`samber-mo/sub-SKILL.md`** — Monadic types for Golang using samber/mo — Option, Result, Either, Future, IO, Task, and State types for type-safe nullable values, error handling, and functional composition with pipeline sub-packages. Apply when using or adopting samber/mo, when the codebase imports `github.com/samber/mo`, or when considering functional programming patterns as a safety design for Golang.

- **`samber-ro/sub-SKILL.md`** — Reactive streams and event-driven programming in Golang using samber/ro — ReactiveX implementation with 150+ type-safe operators, cold/hot observables, 5 subject types (Publish, Behavior, Replay, Async, Unicast), declarative pipelines via Pipe, 40+ plugins (HTTP, cron, fsnotify, JSON, logging), automatic backpressure, error propagation, and Go context integration. Apply when using or adopting samber/ro, when the codebase imports github.com/samber/ro, or when building asynchronous event-driven pipelines, real-time data processing, streams, or reactive architectures in Go. Not for finite slice transforms (→ See `samber-lo/sub-SKILL.md` skill).

- **`samber-hot/sub-SKILL.md`** — In-memory caching in Golang using samber/hot — eviction algorithms (LRU, LFU, TinyLFU, W-TinyLFU, S3FIFO, ARC, TwoQueue, SIEVE, FIFO), TTL, cache loaders, sharding, stale-while-revalidate, missing key caching, and Prometheus metrics. Apply when using or adopting samber/hot, when the codebase imports github.com/samber/hot, or when the project repeatedly loads the same medium-to-low cardinality resources at high frequency and needs to reduce latency or backend pressure.

- **`samber-oops/sub-SKILL.md`** — Structured error handling in Golang with samber/oops — error builders, stack traces, error codes, error context, error wrapping, error attributes, user-facing vs developer messages, panic recovery, and logger integration. Apply when using or adopting samber/oops, or when the codebase already imports github.com/samber/oops.

- **`samber-slog/sub-SKILL.md`** — Structured logging extensions for Golang using samber/slog-* packages — multi-handler pipelines (slog-multi), log sampling (slog-sampling), attribute formatting (slog-formatter), HTTP middleware (slog-fiber, slog-gin, slog-chi, slog-echo), and backend routing (slog-datadog, slog-sentry, slog-loki, slog-syslog, slog-logstash, slog-graylog...). Apply when using or adopting slog, or when the codebase already imports any github.com/samber/slog-* package.

### API / RPC / schemas

- **`grpc/sub-SKILL.md`** — Provides gRPC usage guidelines, protobuf organization, and production-ready patterns for Golang microservices. Use when implementing, reviewing, or debugging gRPC servers/clients, writing proto files, setting up interceptors, handling gRPC errors with status codes, configuring TLS/mTLS, testing with bufconn, or working with streaming RPCs.

- **`graphql/sub-SKILL.md`** — Implements GraphQL APIs in Golang using gqlgen or graphql-go. Apply when building GraphQL servers, designing schemas, writing resolvers, handling subscriptions, or integrating GraphQL with existing Go HTTP services. Also apply when the codebase imports `github.com/99designs/gqlgen` or `github.com/graph-gophers/graphql-go`.

- **`swagger/sub-SKILL.md`** — Golang OpenAPI/Swagger documentation with swaggo/swag — annotation comments (@Summary, @Param, @Success, @Router, @Security), swag init code generation, framework integrations (gin, echo, fiber, chi, net/http), security definitions (Bearer/JWT, OAuth2, API key), and struct tags (swaggertype, enums, example, swaggerignore). Apply when adding or maintaining Swagger/OpenAPI docs in a Go project, or when the codebase imports github.com/swaggo/swag, github.com/swaggo/gin-swagger, github.com/swaggo/echo-swagger, github.com/swaggo/http-swagger, or github.com/swaggo/files.

### Operations

- **`observability/sub-SKILL.md`** — Golang everyday observability — the always-on signals in production. Covers structured logging with slog, Prometheus metrics, OpenTelemetry distributed tracing, continuous profiling with pprof/Pyroscope, server-side RUM event tracking, alerting, and Grafana dashboards. Apply when instrumenting Go services for production monitoring, setting up metrics or alerting, adding OpenTelemetry tracing, correlating logs with traces, migrating legacy loggers (zap/logrus/zerolog) to slog, adding observability to new features, or implementing GDPR/CCPA-compliant tracking with Customer Data Platforms (CDP). Not for temporary deep-dive performance investigation (→ See `benchmark/sub-SKILL.md` and `performance/sub-SKILL.md` skills).

- **`database/sub-SKILL.md`** — Comprehensive guide for Go database access — parameterized queries, struct scanning, NULLable columns, transactions, isolation levels, SELECT FOR UPDATE, connection pool, batch processing, context propagation, and migration tooling. Use when writing, reviewing, or debugging Golang code that interacts with PostgreSQL, MariaDB, MySQL, or SQLite; for database testing; or for questions about database/sql, sqlx, or pgx. Does NOT generate database schemas or migration SQL.

- **`performance/sub-SKILL.md`** — Golang performance optimization patterns and methodology - if X bottleneck, then apply Y. Covers allocation reduction, CPU efficiency, memory layout, GC tuning, pooling, caching, and hot-path optimization. Use when profiling or benchmarks have identified a bottleneck and you need the right optimization pattern to fix it. Also use when performing performance code review to suggest improvements or benchmarks that could help identify quick performance gains. Not for measurement methodology (→ See `benchmark/sub-SKILL.md` skill) or debugging workflow (→ See `troubleshooting/sub-SKILL.md` skill).

- **`security/sub-SKILL.md`** — Security best practices and vulnerability prevention for Golang. Covers injection (SQL, command, XSS), cryptography, filesystem safety, network security, cookies, secrets management, memory safety, and logging. Apply when writing, reviewing, or auditing Go code for security, or when working on any risky code involving crypto, I/O, secrets management, user input handling, or authentication. Includes configuration of security tools.

- **`safety/sub-SKILL.md`** — Defensive Golang coding to prevent panics, silent data corruption, and subtle runtime bugs. Use when encountering nil panics, append aliasing, map concurrent access, float comparison pitfalls, or zero-value design questions. Also use when reviewing code for nil-safety, numeric conversion overflow, resource lifecycle issues (defer in loops), or defensive copying of slices and maps.

- **`modernize/sub-SKILL.md`** — Modernize Golang code to use recent language features, standard library improvements, and idiomatic patterns. Trigger proactively when writing or reviewing Go code and old-style patterns are detected, or when encountering a deprecation warning. Also use when the user explicitly asks for modernization, a Go version upgrade, or a CI/tooling refresh.

- **`troubleshooting/sub-SKILL.md`** — Troubleshoot Golang programs systematically - find and fix the root cause. Use when encountering bugs, crashes, deadlocks, or unexpected behavior in Go code. Covers debugging methodology, common Go pitfalls, test-driven debugging, pprof setup and capture, Delve debugger, race detection, GODEBUG tracing, and production debugging. Start here for any 'something is wrong' situation. Not for interpreting profiles or benchmarking (→ See `benchmark/sub-SKILL.md` skill) or applying optimization patterns (→ See `performance/sub-SKILL.md` skill).

- **`popular-libraries/sub-SKILL.md`** — Recommends production-ready Golang libraries and frameworks. Apply when the user explicitly asks for library suggestions, wants to compare alternatives, needs to choose a library for a specific task, or when a new dependency is being added to the project.

- **`stay-updated/sub-SKILL.md`** — Provides resources to stay updated with Golang news, communities and people to follow. Use when seeking Go learning resources, discovering new libraries, finding community channels, or keeping up with Go language changes and releases.

- **`how-to/sub-SKILL.md`** — Golang skills orchestrator — always active on any Golang coding, review, debug, or setup task. Reads the task context and loads the most relevant skills, often multiple at once: writing a gRPC service loads grpc + testing + error-handling; debugging a panic loads troubleshooting + safety; auditing security loads security + lint + safety. Also disambiguates competing clusters when two sub-skills seem to overlap (performance vs benchmark vs troubleshooting, samber-lo vs mo vs ro, DI cluster, safety vs security).

## Upstream attribution

Original work © @samber, licensed MIT. See `LICENSE` for full text and
the upstream repository [samber/cc-skills-golang](https://github.com/samber/cc-skills-golang)
for the source, methodology (`GOLANG-AI-DRIVEN-REVIEW.md`), and the
published evaluation uplifts (`EVALUATIONS.md`).
