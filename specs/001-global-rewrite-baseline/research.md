# Research and architecture decisions

Date: 2026-10-11; status: PROPOSED / evidence gates OPEN. Source spec 53b8c117;
constitution 1.1.0. “Selected” here means selected for this draft, not owner approval
or evidence from a live deployment. Public primary sources only; no provider/bot calls.

## ADR-001 — One inward-dependency modular release

Decision: one Python package/image, ingress and metadata worker, seven contexts
from plan.md. PostgreSQL-backed claims/outbox avoid an extra broker. Ingress performs
bounded async conversation/classifier work inline; no raw cross-process payload bus.
Worker executes regenerable metadata jobs and safety reconciliation.
Alternatives: microservices/broker add operations without established need; all
work in a process-local queue violates accepted durability. Framework web ingress
is unnecessary for initial polling. Exceptions/framework objects stay in adapters.

## ADR-002 — Supported async stack

CPython 3.14 standard GIL proposed: bugfix-supported versus security-only3.13 and
just-released3.15. [Python lifecycle](https://devguide.python.org/versions/).
PTB 22.8 metadata accepts HTTPX>=0.27,<0.29 and conditionally httpcore>=1.0.9 on
Python>=3.14; HTTPX 0.28.1 candidate. Prefer minimal Bot API dependency, no persistence,
webhook/job-queue/all extras. [PTB metadata](https://github.com/python-telegram-bot/python-telegram-bot/blob/v22.8/pyproject.toml).
PG 18 supported line (18.6 observed), SQLAlchemy 2.0 async + Psycopg 3 and dedicated
Alembic. AsyncSession is per concurrent unit of work, never shared across tasks.
[PG lifecycle](https://www.postgresql.org/support/versioning/),
[SQLAlchemy asyncio](https://docs.sqlalchemy.org/en/20/orm/extensions/asyncio.html),
[Alembic asyncio](https://alembic.sqlalchemy.org/en/latest/cookbook.html#using-asyncio-with-alembic).
Exact patch/digest/transitive locks/platform wheels and license/security proof are
later verified release identities, not fabricated now.

## ADR-003 — Explicit polling admission

Use Bot.get_updates under one DB-fenced lease, application-owned cursor and sanitized
durable acceptance. PTB's default Updater enqueues in RAM then advances offset and
its bootstrap changes webhook state: not the approved durable/cutover boundary.
[Updater source](https://github.com/python-telegram-bot/python-telegram-bot/blob/v22.8/src/telegram/ext/_updater.py).
Telegram confirms on a higher subsequent offset; updates retain at most24h and IDs
may randomize after a week idle. Do not equate update_id with paid identity.
[Telegram update methods](https://core.telegram.org/bots/api#getting-updates),
[Update](https://core.telegram.org/bots/api#update).
No automatic deleteWebhook, pending-drop or negative-offset recovery. Existing
webhook/conflicting poller is an unready condition requiring separate handover authority.
Idle cursor/bootstrap behavior is a required future contract test, not proved runtime.

## ADR-004 — Privacy storage is physically separate

Metadata-only encrypted backups/WAL/replicas may contain ledger, policy, consent,
tombstones and outcomes, never Q&A/context/raw examples/provider bodies. Select
separate physical metadata and content clusters. Two databases/schemas in one
cluster share physical WAL/base backups. Logical selective replication adds DDL/
replica-identity/failover machinery and is deferred. [PG physical backup](https://www.postgresql.org/docs/18/continuous-archiving.html).
Content has no backups/archive/replication/snapshots. SQL deletion alone does not
prove erasure of MVCC/WAL/filesystem remnants: a verified content encryption/key
destruction and storage-erasure lifecycle is REQUIRED before persistence is enabled.
No particular key service or purported crypto-erasure is invented; gateG-03 remainsOPEN.
[PG MVCC](https://www.postgresql.org/docs/18/mvcc.html).
Transient-only opt-out payload is not placed in durable work, disk spill, logs or
crash dumps. Surviving metadata permits safe terminal/review disposition, not
reconstruction of a lost answer. Eligible opt-in content still enforces all-owner
source dependencies and minimum expiry; access-time checks precede export/context.

## ADR-005 — Critical durability trades availability for safety

Proposed envelope: single metadata-primary process/storage/host loss OR partition,
with independently placed synchronized durable standby available, valid promotion
fencing and surviving checkpoint/control evidence. Critical commits require local
fsync plus named standby durable flush; no async/local-only fallback. Promotion
requires proof it includes every acknowledged critical checkpoint and former writer
is fenced. New critical admissions/effects stop when proof/durable standby is absent.
[PG synchronous replication](https://www.postgresql.org/docs/18/warm-standby.html#SYNCHRONOUS-REPLICATION).
Simultaneous destruction/corruption of both domains is outside this proposedRPO0
envelope; disclose it and fail closed, never recover stale authority/ledger as PASS.
After promotion, do not acknowledge new critical writes/effects until a verified
independent synchronous peer and witness/fencing contract are re-established.
A surviving single copy permits controlled reconciliation, not pretending two-copy
RPO0 is still available. Remote WAL of METADATA only, encrypted backups <=30d,
tested restore.
Same-host reference standby tests protocol, NOT independent host-loss resilience.
Independent placement, fencing, archival capacity and cost are not existing server facts.

## ADR-006 — Required initial Flowise is currently blocked

Official maintainer announced code freeze2026-07-29, archive2026-08-13 and
EOL2026-08-31. [Official announcement](https://github.com/FlowiseAI/Flowise/discussions/6727).
Pinned upstream non-streaming implementation persists both question and answer.
Empty history/fresh session is not no-storage proof. [Pinned source](https://github.com/FlowiseAI/Flowise/blob/9291856d1ea4a4ceea9f8fef8ce14f4f6c81e8eb/packages/server/src/utils/buildChatflow.ts).
Inference: ordinary upstream Prediction cannot be represented as meeting approved
transient-only provider processing. Real integration remains disabled; design the
Flowise port/adapter without claiming usable compliance. Closing requires an
explicitly approved maintained compliant deployment/fork scope plus actual technical
and contractual evidence, OR owner-approved spec/constitution amendment to another
provider. Neither is selected. No privacy relaxation or silent replacement.
[Prediction contract](https://docs.flowiseai.com/api-reference/prediction).

## ADR-007 — Egress and secret boundaries

Fixed-purpose gateway is sole outward-network member; route fixed Telegram methods
and separately approved provider mapping only. No arbitrary destination/CONNECT.
Docker internal networks are routing isolation, not domain ACLs.
[Compose networks](https://docs.docker.com/compose/how-tos/networking/).
Gateway implementation/image and enforceable DNS/IP/redirect/TLS policy require
reviewed selection and adversarial proof before plan approval (G-04); a generic
proxy or handwritten claim is not closure. Secrets are runtime service-scoped
injection; Compose file mounts are not an encrypted secret manager.
[Compose secrets](https://docs.docker.com/compose/how-tos/use-secrets/).
No privileged gateway or host-global firewall/tool installation is assumed.

## ADR-008 — Schema/import/rollback authority

New DB schema is designed independently, Alembic-only, dedicated coordinated job.
Static old models/migrations disagree; actual sanitized catalog/applied revisions
and entity totals are required, never guessed from source. See migration-plan.md.
Rollback after V2 changes cannot safely resume old balances/denials/grant/outcome
state. Select compatible V2 image over canonical critical metadata, or no-effect
safe state. Old deployment freeze/route transfer needs separate owner-controlled
handover authority consistent with source immutability; never silently stop it.
No import SQL, DDL, migration revision or server command is created/executed.

## ADR-009 — Reproducible bounded delivery

Distinct runtime/migrate/ops/developer tooling, immutable input identities, complete
wheel hashes, SBOM/provenance/scans, project-only Compose and finite total resources.
[Docker multi-stage](https://docs.docker.com/build/building/multi-stage/),
[pip secure installation](https://pip.pypa.io/en/stable/topics/secure-installs/).
No mutable latest, hidden dependency/test resources or trusted unreviewed artifact.
No paid provisioning is authorized; actual all-in monthly cap/priced manifest remainsG-05.

## Open evidence, not inferred answers

G-01 real Flowise maintenance/privacy/processor agreement; G-02 actual sanitized
source schema/maps/counts/current authority; G-03 content erasure; G-04 egress identity
and enforcement; G-05 priced independent fault domains/cost/platform/regions;
G-06 actual corpus/locks/security/test/recovery evidence; G-07 owner plan approval;
G-08 separately authorized handover/server operations. See plan-review.md.
These gates can support an honest draft but cannot earn full Speckit/constitution PASS.
