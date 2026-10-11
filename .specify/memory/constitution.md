<!--
Sync Impact Report

- Version change: 1.0.0 -> 1.1.0
- Modified principles:
  - II. Modular, Provider-Neutral Architecture: bounded contexts, ownership, and
    dependency rules made explicit.
  - IV. Security, Privacy, and Least Privilege: legacy-secret response,
    AI-provider boundaries, and supply-chain controls expanded.
  - V. Data Integrity and Migration Discipline: schema-drift detection,
    single-run migration execution, legacy import, and cutover controls expanded.
  - VI. Test-First Quality Gates -> VII. Test-First Quality Gates: renumbered and
    expanded with legacy characterization, container, and migration test gates.
  - VII. Observability, Internationalization, and Operability ->
    VIII. Observability, Internationalization, and Operability: renumbered.
- Added principle:
  - VI. Container Isolation and Reproducible Delivery
- Added content:
  - Legacy Risk Baseline under Global Product and Engineering Constraints
- Removed sections: None
- Template compatibility:
  - Core templates remain structurally compatible.
  - Generated specifications, plans, and task lists MUST carry the new Docker,
    migration, security, and test obligations when their workflows are run.
- Follow-up placeholders: None
-->

# AI Bot V2 Global Rewrite Constitution

## Core Principles

### I. Behavioral Parity and Traceability (NON-NEGOTIABLE)

`ai_bot6` is an immutable behavioral reference. It MUST remain read-only: no file,
branch, tag, issue, pull request, configuration, or deployment belonging to that
repository may be changed as part of this rewrite.

Before implementation, every discovered legacy behavior MUST be mapped to a
versioned requirement and classified as **preserve**, **intentionally change**, or
**retire**. Each mapping MUST identify its user or operator, acceptance scenarios,
security and data implications, and verification method. Intentional changes and
retirements require explicit user approval and a migration or communication plan.
Unmapped behavior MUST NOT be silently omitted, and undocumented legacy behavior
MUST be characterized with tests or reproducible observations before replacement.

The new system MUST preserve approved externally observable contracts, including
Telegram commands and callbacks, authorization outcomes, configuration precedence,
stored-data semantics, scheduled-job effects, and operator-visible telemetry.
Compatibility does not require retaining legacy internal structure.

### II. Modular, Provider-Neutral Architecture

The system MUST separate domain rules, application use cases, ports, infrastructure
adapters, and platform composition. Dependencies MUST point inward: domain and
application code MUST NOT import Telegram, Flowise, SQLAlchemy, HTTP clients,
monitoring SDKs, or deployment-specific modules.

The initial bounded contexts MUST separate at least Conversation and AI, AdsGuard,
Identity and Administration, Tokens and Ledger, Configuration and Tenancy,
Observability, and Platform Operations. Each context MUST own its terminology,
invariants, public contracts, and data access. Cross-context interaction MUST use an
application-level port, a versioned event, or another documented contract; direct
access to another context's tables or private modules is prohibited. Any dependency
that crosses these boundaries requires an architecture decision record.

Telegram transport, AI providers, persistence, scheduling, localization, secrets,
and observability MUST be replaceable adapters behind explicit, versioned
interfaces. Handlers MUST translate input and invoke application services; they
MUST NOT contain raw SQL, provider-specific orchestration, authorization policy, or
business rules. Provider selection MUST support Flowise initially without making
Flowise the domain model.

Modules MUST have one cohesive responsibility. A production module exceeding 500
logical lines or spanning more than one architectural layer requires a documented
decomposition rationale and reviewer approval. Cyclic dependencies are prohibited.
Public contracts MUST be typed and documented, and breaking contract changes MUST
follow the governance rules below.

Transaction boundaries MUST be owned by application use cases. Repository
interfaces MUST expose intent rather than storage details, and database sessions,
Telegram update objects, provider payloads, environment variables, and framework
exceptions MUST NOT leak into the domain. Cross-context side effects that cannot be
committed atomically MUST use an outbox, inbox, or equivalently durable pattern with
explicit delivery and deduplication semantics.

### III. Async Reliability and Resilience

Runtime I/O MUST be asynchronous and MUST NOT block the event loop. Every network,
database, and provider call MUST have an explicit timeout, cancellation behavior,
bounded concurrency, and a documented failure policy. Retries MUST be bounded,
use backoff with jitter, distinguish transient from permanent failure, and avoid
retry amplification.

Message handling, token spending, weekly grants, pending AdsGuard actions, album
processing, callbacks, and scheduled jobs MUST be idempotent. Multi-instance work
MUST use atomic claims, ownership leases or equivalent fencing, deterministic
deduplication keys, and safe stuck-work recovery. Processes MUST support graceful
shutdown, readiness and liveness checks, and recovery without duplicate user-visible
effects.

Errors MUST be surfaced as typed outcomes, actionable telemetry, or explicit
user-safe responses. Silent exception swallowing, unbounded queues, infinite retry
loops, and in-memory-only coordination for durable work are prohibited.

### IV. Security, Privacy, and Least Privilege

Access MUST be denied by default. Superadmin, group-admin, tenant, private-message,
and feature-level permissions MUST be enforced in application services as well as
at transport entry points. Every read and write of tenant data MUST carry and verify
an explicit tenant or chat scope. Privileged operations MUST create tamper-evident,
redacted audit records containing actor, scope, action, result, and correlation ID.

All external input—including commands, callback payloads, deep links, message
content, provider responses, configuration, and imported legacy data—MUST be
validated with bounded sizes and allowlisted formats. SQL MUST be parameterized;
dynamic identifiers MUST be allowlisted. Secrets MUST come from an approved secret
store or deployment injection, MUST never be committed or logged, and MUST support
rotation. Logs, metrics, traces, errors, fixtures, and support exports MUST exclude
tokens, credentials, raw private conversations, and unnecessary personal data.

Every credential, token, password, private key, or credential-like value ever
committed to `ai_bot6` or another source repository MUST be treated as compromised.
It MUST NOT be copied into the new project or any environment; its owner MUST revoke
or rotate it and record completion before the affected integration is enabled.
Repository history, build contexts, images, artifacts, and release bundles MUST be
secret-scanned in CI. A detected secret blocks release and triggers the incident
response procedure without reproducing the secret in logs or tickets.

Each approved plan MUST include a threat model covering authentication and
authorization, tenant isolation, prompt and provider abuse, callback tampering,
injection, SSRF, replay and race conditions, denial of service, supply-chain risk,
backup exposure, and administrative misuse. Dependencies and container images MUST
be locked or pinned to reviewed versions, continuously scanned, and accompanied by a
software bill of materials for releases. Package provenance, integrity hashes, and
licenses MUST be verified; critical or known-exploited vulnerabilities block release
unless a time-bounded governance exception supplies compensating controls.

AI-provider adapters MUST treat prompts, retrieved context, tool calls, URLs, and
model output as untrusted data. The application MUST enforce tool allowlists, URL
and network egress policy, output validation, data-loss prevention boundaries, and
per-tenant quotas outside the model. Provider prompts and responses MUST NOT grant
authorization or select a tenant scope, and sensitive content MUST NOT be sent to a
provider without an approved data-processing and retention policy.

Personal-data collection MUST be minimized and assigned documented retention,
export, correction, and deletion behavior. Privacy-impacting changes and security
exceptions require explicit approval before implementation.

### V. Data Integrity and Migration Discipline

There MUST be one authoritative, reviewable schema contract. Alembic migrations are
the only permitted mechanism for production schema changes; runtime code MUST NOT
create, infer, or silently repair schema. ORM models, repository queries, migrations,
fixtures, imports, and documentation MUST be checked for contract consistency in CI.
Applied migrations MUST be immutable and checksum-verifiable. The migration graph
MUST have one deployable head unless a reviewed merge migration explicitly joins
temporary branches.

Every schema change MUST define forward migration, compatibility window, rollback
or forward-recovery strategy, backup and restore procedure, data validation, and
reconciliation queries. Migrations MUST be tested from a supported production-like
snapshot and from a clean database. Destructive migrations require a staged
expand-migrate-contract process and explicit approval.

Production application startup MUST NOT execute migrations. A release MUST use one
dedicated, observable migration job that performs compatibility and capacity
preflight checks, acquires a database advisory lock or equivalent single-run fence,
records the exact revision, and exits before incompatible application instances
start. Re-running a migration MUST be safe, and partial failure MUST have a tested
recovery procedure. Two services MUST NOT race to upgrade the same database.

Schema changes used during a rolling deployment MUST support the old and new
application versions throughout an explicit compatibility window. Expand, bounded
backfill, validation, application cutover, and contract MUST be separate phases when
an in-place change is not backward-compatible. Backfills MUST be resumable,
idempotent, rate-limited, observable, and isolated from request latency. Unsafe
downgrades MUST use tested forward recovery or backup restoration rather than a
pretend rollback.

CI MUST build the database from zero, upgrade every supported prior revision to
head, validate a sanitized production-like snapshot, and compare the resulting
schema with the authoritative contract. Repository queries MUST execute against the
resulting schema. Baselines that use conditional creation to conceal drift, manual
production DDL, edited applied migrations, and runtime fallbacks for missing columns
are prohibited.

Legacy data MUST enter through a versioned import pipeline and isolated staging
area. Imports MUST validate type, ownership, tenant scope, encoding, timestamps,
referential integrity, and security classification before promotion. Each import
MUST record source revision, checksums, row counts, rejected rows, reconciliation
results, and an auditable approval. Legacy credentials and secret material MUST be
excluded rather than migrated. A cutover requires a restore-tested backup,
rehearsed duration, acceptance thresholds, abort criteria, rollback or
forward-recovery steps, and post-cutover reconciliation.

Financial-like token balances, grants, spends, and deduplication records MUST use
transactions, immutable ledger semantics, database constraints, and auditable
idempotency keys. Concurrency invariants MUST be enforced by the database rather
than process-local assumptions. Timestamps MUST be timezone-aware and stored in UTC;
local presentation MUST use the user's or tenant's configured timezone.

### VI. Container Isolation and Reproducible Delivery (NON-NEGOTIABLE)

Production images MUST be deterministic, minimal, multi-stage builds. Base images,
release images, CI actions, system packages, and language dependencies MUST be
pinned by immutable digest, hash-verified lock, or equivalent reviewed identity.
Mutable tags such as `latest` are prohibited in release inputs. Every release image
MUST have provenance, an SBOM, vulnerability and malware scan results, and an
immutable digest used by deployment.

Runtime, migration, administrative tooling, and developer tooling MUST use distinct
images, stages, or Compose profiles with least privilege for their purpose.
Specification tools such as Specify CLI MUST remain project-local and MUST NOT be
installed on the host globally or included in production images. Build contexts
MUST exclude repository history, secrets, environment files, dumps, logs, backups,
virtual environments, caches, and unrelated services.

Every production container MUST run as a fixed non-root UID and GID, drop all Linux
capabilities before adding an explicitly justified minimum, set
`no-new-privileges`, and use a read-only root filesystem. Required writable paths
MUST be explicit `tmpfs` or dedicated volumes with size, ownership, retention, and
backup rules. Privileged mode, host networking, device passthrough, Docker socket
mounts, and unrestricted host bind mounts are prohibited unless a time-bounded
governance exception names the threat controls.

Compose resources MUST be project-scoped. Global `container_name` values,
hard-coded host paths, hostnames, domains, chat identifiers, and shared external
networks are prohibited unless the approved deployment plan documents the
integration contract and collision controls. Only required ports may be published,
each on an explicit interface. Internal services MUST use internal networks and
deny unnecessary egress. Mounts MUST be minimal, explicitly read-only unless writes
are required, and limited to project-owned or approved persistent-data paths.

Secrets MUST be injected at runtime through an approved secret manager, Docker
secret, or read-only secret file. They MUST NOT appear in Compose files, committed
environment files, image layers, build arguments, labels, command lines, health
checks, or logs. Deployments MUST define rotation without rebuilding the application
image.

Containers MUST declare bounded CPU, memory, process, file-descriptor, and log
resources; graceful-stop behavior; and health checks that distinguish startup,
liveness, and readiness. Application containers MUST NOT perform schema upgrades in
their entrypoint. Persistent services MUST have tested backup, restore, upgrade, and
volume-ownership procedures.

CI MUST validate Dockerfiles and resolved Compose configuration, build without
undeclared context, inspect effective user and security options, scan images and
secrets, verify SBOM and provenance, and run smoke tests under the production
security settings. A release MUST be reproducible from reviewed source and locks
without relying on mutable host state.

### VII. Test-First Quality Gates (NON-NEGOTIABLE)

No production implementation may begin until the user has approved both the feature
specification and architecture plan. For each approved behavior, executable
acceptance tests or contract tests MUST be written first and observed failing for
the intended reason before implementation proceeds. Work follows the
red-green-refactor cycle; defect fixes MUST begin with a reproducing regression test.

The test architecture MUST include:

- read-only characterization tests or reproducible fixtures for every legacy
  behavior classified as preserve or intentionally change;
- unit tests for domain rules and application services;
- contract tests for Telegram transport, AI-provider ports, repositories, and
  configuration contracts;
- integration tests against a real supported PostgreSQL version and applied
  migrations;
- end-to-end tests for critical chat, administration, AdsGuard, token, localization,
  and recovery journeys;
- authorization, tenant-isolation, malicious-input, secret-redaction, and other
  security tests;
- concurrency, idempotency, retry, scheduled-job, and multi-instance recovery tests;
- clean-install, every-supported-upgrade-path, schema-contract, backfill,
  backup-restore, failed-migration-recovery, and legacy-data reconciliation tests;
- Docker build, resolved-Compose, non-root, read-only-filesystem, health-check,
  network-isolation, resource-limit, secret-absence, and graceful-shutdown tests.

Tests MUST be deterministic, isolated, parallel-safe, and independent of live
Telegram or AI-provider accounts. CI MUST block merges on test, lint, format,
type-check, migration-consistency, Docker-policy, dependency, secret, static
analysis, and security-scan failures. Critical integrations MUST use controlled
fakes for contract tests and real disposable dependencies for integration tests;
SQLite or mocked SQL MUST NOT substitute for PostgreSQL migration validation.
Application code MUST maintain at least 80 percent line coverage; authorization,
tenant isolation, token ledger, migration, and AdsGuard action code MUST maintain at
least 90 percent branch coverage. Coverage MUST NOT decrease without an approved,
time-bounded exception, and coverage alone never substitutes for behavior-focused
tests.

Flaky tests MUST be treated as defects, assigned an owner, and fixed or quarantined
with an expiry and linked issue; rerunning CI until it passes is prohibited. Test
fixtures MUST contain synthetic or irreversibly anonymized data and MUST NOT contain
production messages, identifiers, credentials, or backups.

### VIII. Observability, Internationalization, and Operability

Every request, update, job, and provider call MUST carry a correlation ID and emit
structured, redacted logs plus relevant metrics and traces. Plans MUST define
service-level indicators and objectives for latency, availability, error rate,
queue age, provider health, AdsGuard action delay, and job success. Alerts MUST be
actionable, tested, and linked to runbooks. Health endpoints MUST distinguish
liveness from readiness and MUST verify critical dependencies without leaking
sensitive details.

All user-visible text MUST come from versioned localization catalogs. The system
MUST support Unicode, pluralization, RTL and LTR layouts, locale-aware formatting,
and tenant or user language selection with an explicit fallback chain. Missing or
stale translations MUST be detectable in CI; production behavior MUST NOT silently
fall back to developer-only strings.

Every release MUST include deployment, rollback, backup, restore, incident-response,
and data-reconciliation instructions. Operational configuration changes MUST be
validated, auditable, reversible, and observable without requiring a code release.

## Global Product and Engineering Constraints

### Legacy Risk Baseline

The read-only review of `ai_bot6` establishes the following evidence-based risk
baseline:

- runtime queries and historical migrations disagree on multiple table names,
  columns, keys, and row models, including session, history, administration, and
  ledger-related data;
- the application startup path and a separate migration service can both invoke
  schema upgrades, creating an avoidable race and unclear ownership;
- large modules combine transport, policy, orchestration, SQL, configuration, and
  presentation concerns;
- automated verification is incomplete: the existing security workflow covers a
  narrow SQL-safety subset while unit, integration, migration, type, and container
  gates are not a complete enforced baseline;
- dependencies and service images include mutable or incompletely locked versions,
  and deployment configuration contains environment-specific host paths, domains,
  identifiers, and shared-network assumptions;
- intended container hardening and effective Compose settings do not consistently
  match; and
- credential-like deployment data has appeared in tracked configuration.

These findings are migration risks, not design patterns to preserve. Each MUST map
to a specification requirement, architecture control, verification task, and
cutover acceptance criterion before production release. The plan MUST maintain a
risk register with owner, severity, evidence, remediation, validation, and closure
status. A risk is not closed by copying, renaming, suppressing, or documenting the
legacy behavior alone.

### Product and Configuration Baseline

The initial compatibility inventory MUST cover, at minimum:

- private and group AI conversations, `/ask`, sessions, history, feedback, unknown
  questions, typing behavior, and conversation cleanup;
- group and private administration, active-group context, deep links, superadmin and
  group-admin roles, DM allowlisting, command menus, and audit trails;
- AdsGuard classification, thresholds, few-shot examples, reply, forward, media and
  caption policy, allowlists, warnings, deletion, mute behavior, pending actions,
  album handling, simulation, statistics, and recovery;
- token wallets, weekly grants, spending, ledgers, deduplication, and feature gates;
- database-first global and per-chat configuration with deployment fallback;
- localization, structured logging, metrics, health checks, alerting, and optional
  error reporting.

This inventory is a discovery floor, not a complete specification. Repository and
runtime evidence MUST decide the final scope. Hard-coded chat IDs, user IDs, domains,
host paths, credentials, locales, providers, or deployment environments are
prohibited in production code.

Configuration MUST be typed, validated at startup and update time, documented, and
resolved through the explicit precedence chain: tenant or chat override, global
database value, deployment-provided value, then safe built-in default. Security
controls MUST fail closed when configuration is missing or invalid. Configuration
changes MUST record actor, prior value, new value, scope, and result without exposing
secrets.

The product MUST be deployable across regions, locales, timezones, Telegram tenants,
AI providers, and supported PostgreSQL environments. Region-specific privacy,
retention, and data-residency requirements MUST be expressible without forks.
Capacity budgets, supported versions, service-level objectives, and cost limits MUST
be defined and validated in the architecture plan.

Generated caches, bytecode, local secrets, database dumps, runtime logs, and editor
artifacts MUST NOT be tracked. Dependencies, migrations, localization catalogs,
contracts, decisions, and operator documentation are reviewed source artifacts.

## Specification-First Delivery Workflow

Delivery MUST proceed through the following gates in order:

1. **Read-only discovery**: characterize `ai_bot6`, produce a behavior and data
   inventory, identify ambiguities and risks, and record reproducible evidence.
2. **Constitution**: establish the binding standards in this document.
3. **Specification**: use the Spec Kit specification workflow to define user
   journeys, preserve/change/retire mappings, requirements, acceptance scenarios,
   edge cases, security and privacy needs, and measurable success criteria.
4. **Specification approval**: the user MUST explicitly approve the specification;
   unresolved decisions block planning.
5. **Architecture plan**: use the Spec Kit planning workflow to produce research,
   bounded contexts, component architecture, data model, contracts, threat model,
   test architecture, observability design, Docker image and Compose topology,
   supply-chain controls, network and secret boundaries, migration ownership,
   legacy-import and rollback strategy, capacity assumptions, and phased delivery
   plan.
6. **Plan approval**: the user MUST explicitly approve the architecture and migration
   plan; unresolved constitution violations block task generation.
7. **Task generation**: use the Spec Kit task workflow to create dependency-ordered,
   independently verifiable tasks. Test, contract, migration-safety, and security
   tasks MUST precede the implementation they govern.
8. **Implementation**: begin only within the approved scope, in small reversible
   vertical slices, with continuous traceability to requirements and tests.

Each pull request MUST identify requirements and tasks served, tests added, security
and data impact, image or deployment impact, observability changes, rollout and
rollback steps, and any approved exceptions. Reviews MUST reject unrelated
refactors, undocumented behavior changes, schema drift, mutable release inputs,
hard-coded deployment assumptions, and changes to `ai_bot6`.

A feature is ready for implementation only when its acceptance scenarios, data and
permission boundaries, external contracts, container and network boundaries,
migration path, failure behavior, test strategy, and approval status are explicit.
A feature is done only when its approved behavior, tests, telemetry, localization,
security checks, image and Compose policy checks, migration validation,
documentation, deployment procedure, and rollback procedure are complete.

## Governance

This constitution supersedes conflicting project conventions, templates, plans,
tasks, and implementation preferences. Every specification, plan, task list, review,
and release MUST include an explicit constitution-compliance check. Complexity and
exceptions require written justification proportional to risk.

Amendments require a reviewed proposal stating the rationale, affected principles,
compatibility impact, migration needs, and updates required in dependent artifacts.
They require explicit project-owner approval before adoption. Constitution versions
follow semantic versioning:

- **MAJOR** for incompatible governance changes or removal or redefinition of a
  principle;
- **MINOR** for a new principle or materially expanded mandatory guidance;
- **PATCH** for clarifications that do not change obligations.

An exception MUST name an owner, scope, risk, compensating controls, expiry date, and
remediation or rollback plan. Expired exceptions block release. Security, privacy,
tenant-isolation, ledger-integrity, and source-repository immutability rules cannot
be waived without explicit project-owner approval.

Compliance MUST be reviewed at each approval gate and at least once per release.
Where this constitution and a legacy behavior conflict, the conflict MUST be made
visible in the specification and resolved by explicit approval; it MUST never be
settled implicitly in code.

**Version**: 1.1.0 | **Ratified**: 2026-08-17 | **Last Amended**: 2026-08-17
