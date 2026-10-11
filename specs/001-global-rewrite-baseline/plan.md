# Implementation Plan: AI Bot V2 global rewrite baseline

**Branch**: main | **Date**: 2026-10-11 | **Spec**: approved [53b8c117](https://github.com/vahid162/ai_bot_v2/blob/53b8c117d9309fffbfeb41ca2df682fc41e779be/specs/001-global-rewrite-baseline/spec.md)
**Input**: specs/001-global-rewrite-baseline/spec.md and its linked approved contracts.
**Status**: DRAFT; owner plan approval absent. Phase 0 research and Phase 1 design
artifacts are published for review, not a successful implementation/release gate.
Known blocking findings are explicit below. No constitutional waiver is requested or assumed.

## Summary

Rewrite the approved 23 behavior families, 65 requirements and 14 success criteria
as one modular Python release. Telegram is the initial transport. Separate domain,
application ports, adapters and composition; use PostgreSQL transactions and
metadata-only durable work, not process-local coordination. Start with fresh
conversation history/context; conditionally preserve validated legacy wallet/ledger
continuity under separately approved actual mappings. Respect current group mode,
terminal paid identities and UNKNOWN external-result NO_REPLAY holds.

The proposed release has an ingress process and a metadata-job worker from the
same image. Ingress owns one fenced long-poll receiver and bounded transient
conversation/classifier/album payloads. Workers own scheduled metadata work.
Do not introduce Redis, Kafka, Celery, Kubernetes or a public web application.

Two independent PostgreSQL clusters separate critical METADATA from optional
CONTENT. METADATA has a synchronously durable standby; CONTENT has no backups,
archive or replication. This is a privacy/recovery requirement, not a microservice split.
Actual content erasure and independent-domain recovery proof remain approval gates.

Flowise remains the required initial adapter contract, but real integration is
BLOCKED: upstream is archived/EOL and ordinary Prediction persists question/answer.
No replacement provider, fork, privacy exception or server change is selected.

## Technical Context

| Item | Proposed choice / evidence boundary |
| --- | --- |
| Language/version | CPython 3.14, ordinary GIL build; exact patch/platform image digest reviewed at lock gate. |
| Primary dependencies | python-telegram-bot 22.8 Bot API adapter; HTTPX 0.28.1 candidate; SQLAlchemy 2.0 async infrastructure; Psycopg 3 async driver; Alembic dedicated schema tool. Complete transitive compatibility/hashes remain NOT VERIFIED. |
| Storage | PostgreSQL 18 supported line (18.6 observed candidate), metadata primary+sync standby and physically separate content cluster. UTC timestamps, integer ledger amounts, explicit context ownership. |
| Testing | pytest/async support, controlled transport/provider/clock fakes, real disposable PG 18; coverage, Ruff, type and boundary checks. No tests written or run in this planning work. |
| Target platform | Project-scoped Linux OCI containers/Compose; no global Specify install, no shared server integration assumed. Architecture/CPU region must be declared before image locks. |
| Project type | Telegram service, modular monolith, async ingress plus metadata worker. No HTTP public ingress or FastAPI required initially. |
| Performance goals | Exact SC-003/011/012/014 and measurement-contract clocks, independently per run; not actual upstream capacity. |
| Constraints | <=4 CPU equivalents / 8 GiB evaluated reference budget; default-off consent; 30-day maximum content expiry; metadata-only backups; critical acknowledged RPO0 within approved fault envelope. |
| Scale/scope | Standard50 tenants/1,000 users;2×100/2,000;10800+2160 and21600+4320 unique+duplicate measured offers. Five launch locales. These are selected synthetic references, not measured live traffic. |
| Availability/cost | Proposed production capability availability99.5%/rolling30 days for owner review, defined in operations.md. Current paid-resource authority is0; future monthly all-in ceiling and priced deployment manifest are NOT SET/BLOCKED. |

Versions above are researched candidates, not installed packages, lockfiles or
release identities. No fabricated digest, hosting capacity, legal compliance or
passing benchmark is asserted.

## Constitution Check

Pre-research input: owner spec approval recorded; source immutable; approved behavior
and selected contracts available. Before/after plan hooks absent in approved GitHub tree.

Post-design review:

| Principle | Design coverage | Gate result |
| --- | --- | --- |
| I parity/source immutability | Frozen approved spec/ledger/cases; read-only legacy evidence; no source/server changes. | Design covered; characterization execution NOT RUN. |
| II inward typed boundaries | Seven owning contexts, versioned ports, no handler SQL; initial Flowise contract. | BLOCKED for real Flowise maintenance/privacy compliance; not silently waived. |
| III async/reliability | Bounded I/O, metadata inbox/outbox, claims/fences, unknown holds, current rechecks. | Design covered; race/recovery proof NOT RUN. |
| IV security/privacy | Threat register, egress broker, current scoped authority, default-off consent, split storage. | BLOCKED for actual processing agreement, gateway enforcement and content-erasure proof. |
| V integrity/migration | Alembic-only, dedicated fenced job, immutable revisions, versioned import/reconciliation. | BLOCKED for actual sanitized schema/maps, fault-domain and recovery proof. |
| VI Docker/reproducibility | Distinct runtime/migrate/ops/spec tooling, nonroot scoped topology, exact release identities. | Design covered; effective configuration/locks/scans NOT RUN. |
| VII test-first | No implementation; future acceptance red before code;80%line/90%critical branch. | Approval gate respected; all executable verification NOT RUN. |
| VIII operability/global constraints | Five catalogs, telemetry distinction, SLI/SLO/runbooks, bounded resources. | BLOCKED for priced deployment/cost ceiling and live envelope evidence; new availability target unapproved. |

**ERROR / NOT APPROVAL-READY** where blockers require actual evidence or an explicit
owner-approved amendment. Writing the draft is not PASS, proof of compliance, or
permission to generate tasks/implement. No violations are hidden in complexity tracking.

## Architecture and ownership

| Context | Owns | Public inward contracts |
| --- | --- | --- |
| Identity & Administration | principals, current scoped authority, DM allowance/explicit denial, panels/prompts, admin auditing intents | Authority, scope binding, membership verification |
| Configuration & Tenancy | tenants/topics, typed revisions, precedence, feature/deletion policy | EffectiveConfig, tenant lifecycle, policy snapshots |
| Conversation & AI | unified ask pipeline, sessions/generations, consented owner context, feedback/export | ConversationService, HistoryAccess, AnswerProvider |
| AdsGuard | eligibility/exemptions/classification, guards, albums/member oracle, moderation decisions/examples | Classifier, ModerationDecision, ActionAuthorization |
| Tokens & Ledger | wallets, period outcomes, immutable events/checkpoints, all terminal paid outcomes | GrantEligibility, SettlePaidOutcome, Reconcile |
| Observability | required redacted correlation/audit/evidence, alert production and delivery state | RequiredEvidence, AuditAppender, AlertProduction |
| Platform Operations | admission/cursor/lease, jobs/effects, schema/release/import/recovery | DurableWork, EffectDelivery, SchemaCompatibility |

Cross-context calls go through application ports, not direct private-table queries.
Metadata DB unit-of-work can coordinate registered repositories in one transaction
when an invariant needs atomicity. Ports specify intent, never leak ORM sessions.
Independent content writes use idempotent staged publication, not distributed 2PC.
One bootstrap composition root wires dependencies; domain imports only domain/stdlib.

Processing: bounded physical validation -> reserve volatile capacity -> durable
metadata admission/required evidence and cursor -> unified scoped application
gates -> async transient provider call -> current policy/generation recheck ->
durable effect intent -> external call -> exact confirmed/unknown receipt ->
terminal outcome and (when eligible) warning-only debit. No DB transaction spans
network I/O. Scheduled jobs share identity/clocks/fencing and cannot resurrect
unknown effects or missing text.

## Project Structure

### Documentation (this feature)

- [research.md](research.md): decisions, alternatives, primary evidence and blockers.
- [data-model.md](data-model.md): logical entities, keys, lifecycle and state machines.
- [contracts/application-ports.md](contracts/application-ports.md)
- [contracts/telegram.md](contracts/telegram.md)
- [contracts/provider.md](contracts/provider.md)
- [contracts/import.md](contracts/import.md)
- [migration-plan.md](migration-plan.md)
- [operations.md](operations.md): Docker/security/recovery/capacity/SLI/risks.
- [test-strategy.md](test-strategy.md)
- [quickstart.md](quickstart.md): future validation guide only.
- [plan-review.md](plan-review.md): Persian owner review.
- [approval-record.md](approval-record.md)

Existing spec and constitution remain byte-for-byte frozen. tasks.md is not generated.

### Source Code (repository root) — proposed only, NOT created

Proposed src/guardbot contexts each contain domain, application and adapter modules;
platform/composition owns entrypoints. Proposed tests/{unit,contract,integration,e2e,
migration,security,docker,performance}, migrations/{metadata,content}, locales/{fa,en,
ar,tr,ru}, contracts, docs/runbooks and deployment artifacts are later implementation
outputs, not delivered directories. Separate Alembic environments have one head per
cluster under one reviewed schema contract/release manifest; a dedicated coordinator
fences and records both, runtime verifies its expected revision pair.

## Delivery phases (not tasks)

1. Close documented plan/provider/privacy/capacity/schema gates and obtain explicit
   owner plan approval; only then a separately requested tasks workflow.
2. Test-first foundation: pure ports/domain, synthetic fixtures, metadata/content
   contract, reproducible runtime/migrate isolation, required evidence.
3. Independent safe identity/config/locale/health journeys, then unified conversation
   under verified provider gate; retain consent and late-result protections.
4. AdsGuard warn/none, guarded scheduling/albums, token/grant terminal outcomes;
   deletion remains OFF without separate corpus/evaluation/activation.
5. Actual sanitized import rehearsal, security/concurrency/reference/recovery drills,
   release review; no real import or server operations without new authority.
6. Separately authorized GitHub-first deployment/handover with measured abort/rollback,
   current critical-state reconciliation, and post-cutover review.

Each phase needs traceable red/green evidence; these are sequencing choices, not
task completion claims.

## Complexity Tracking

| Required complexity | Why needed | Simpler option rejected |
| --- | --- | --- |
| Two storage clusters | metadata-only backups must not contain private text/raw examples | Two schemas/DBs in one physical-WAL cluster do not isolate backup content. |
| Synchronous metadata standby + fencing | critical acknowledgedRPO0 for defined single-domain loss | Daily backup/async replica cannot prove no critical acknowledged loss. |
| Fixed-purpose egress boundary | deny unnecessary destinations and SSRF outside the model | Internal Docker networking or a generic HTTPS/CONNECT proxy is not a hostname policy. |
| Durable metadata state + volatile payload | admitted-work safety without unauthorized persistent Q&A | Raw durable inbox/outbox violates history/provider/privacy terms. |

These are justified design costs, not exceptions. Gateway identity, erasure solution
and production fault-domain/cost evidence must be reviewed; no speculative framework
or custom broker implementation is authorized here.

## Speckit execution record

Main agent read speckit-plan and core template, approved spec and constitution.
setup-plan.sh was inspected but NOT RUN: it would write plan.md and a local feature
pointer, contrary to GitHub-only authority. Equivalent feature/path/template
resolution was performed in memory with explicit feature001-global-rewrite-baseline,
destinationmain and core plan-template (no extensions/presets/overrides found).
Documents are generated through GitHub API only; no local pointer, agent-context
file, source or server file is written. Unknowns are explicit blocking gates, not
invented research results. Phase1 draft publication stops before tasks/implementation.
