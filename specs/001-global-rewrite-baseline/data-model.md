# Data model — logical contract, not schema implementation

DRAFT / NOT APPROVED. Names are new target design, not assertions about legacy tables.
No ORM classes, SQL, Alembic revisions or real data are created. All times are
timezone-aware UTC; Telegram IDs use signed64-bit representation; money-like token
amounts are exact integers. Internal IDs are opaque, not hard-coded deployment values.

## Physical stores

METADATA primary+sync standby owns critical operational/evidence/control records;
encrypted metadata-only backup/archival. CONTENT is a separate cluster for optional
eligible text and raw permitted examples; no backup/archive/replica/snapshot.
No content-bearing field may hide in metadata JSON, hashes of guessable text,
diagnostics, callback payload, durable work, audit or backup. Opaque references
identify records but cannot reconstruct erased content. Content encryption/erasure
gate must close before enabling persistence, not just before production.

## Entities, fields, ownership and constraints

| Context / entity | Principal fields | Invariants |
| --- | --- | --- |
| Tenancy: TenantBinding | tenant_id, deployment/bot namespace, external chat_id, status, topic support, revision | unique namespace+chat; explicit tenant scope; closure differs from history clear. |
| Configuration: ConfigRevision | scope, typed key/value, revision, prior revision, actor/action reference, effective_at | validate bounds; CAS updates; tenant→globalDB→deployment→safe default; security invalid/missing fails closed. |
| Identity: PrincipalPolicy | identified actor_id, explicit deny, ordinary allowance sources, revision | deny wins ordinary allowance; unblock only current confirmed private SA; no role/history/token rights implied. |
| Identity: AuthorityEvidence | original actor kind/ID, exact scope/target/action, authority category, verified_at, source revision | ALLOWED/DENIED/UNAVAILABLE; cached role flags never certify fresh Telegram authority. |
| Identity: ScopeBinding | opaque nonce, actor, tenant/topic, operation/target/prompt, issue/idle/absolute expiry, revision | immutable original bindings; 5min prompt,15min panel idle/1habsolute; latest authorization each use. |
| Conversation: PrivacyControl | owner/tenant/topic, consent choice+notice revision, captured generation, clear/context generations, tombstone | default OFF; monotonic critical control; opt-out not clear; loss proof→OFF. |
| Conversation: SessionMetadata | owner/scope, session/context generation, original/closed_at | no old/provider context-bearing session IDs; no content use without eligible current sources. |
| Conversation: Answer/FeedbackMetadata | logical answer ID, exact confirmed chunk IDs, scope, actor vote/report, creation/expiry | no text; vote owner/private or current named groupmember; one immutable scoped vote; ordinary metadata30d. |
| Tokens: Wallet | tenant_id,user_id,balance,revision,checkpoint reference | unique tenant+user; balance>=0; no balance<=carry CHECK (valid above-cap balances retained). |
| Tokens: GrantOutcome | tenant,user,UTC Monday period, GRANTED/SKIPPED_CAP, source identity, event reference | unique tenant+user+period; settled once; only current week; skip and grant both terminal. |
| Tokens: LedgerEvent | event_id, tenant/user, origin, kind, exact delta, proved before/after, original_at, import_at, linked proof, idempotency_key | New V2 events require exact before/after; validated imported historical records preserve only source-proved fields, never invent snapshots. Immutable; grant +1/spend -1; correction linked/proved/current SA; scoped key and atomic new balance/event/outcome. |
| Tokens: LedgerCheckpoint | tenant/user,covered boundary,balance,counts,provenance hash,current-period/safety fence,independent reconciliation | NON_EVENT, never fabricated balancing delta; exact proof precedes detail purge. |
| Tokens: PaidOutcome | source_epoch,tenant,original actor,logical kind/message-or-album ID,outcome,receipt/event ref,settled_at | one terminal identity for DEBIT_CONFIRMED/NO_ACTION/INSUFFICIENT/DISABLED/EXEMPT/NON_AD/GUARD_ONLY/UNKNOWN_HELD; edits/grants cannot reopen. |
| Ads: LogicalItem/MemberEvidence | original identity/date,known member IDs,latest revision nonce,eligibility/classification category/finite score,policy revision | no raw text;<=10album;protected/UNKNOWN members never silently deleted; structure alone not AD. |
| Ads: PendingAction | identity,reason,original due time,policy/current revision references,scope,claim/fence,terminal state | latest authority/mode/content/gate before effect; correction cancels stale structural reason; clocks never reset. |
| Ads: DeletionActivation | approved corpus/evaluation/policy/config/scope revisions,owner approval proof,enabled/revoked revision | OFF default; exact matching independent quality and separate activation; restore/corpus loss never recreates approval. |
| Ads: ExampleMetadata/Allowlist | tenant,approved provenance,label/version/lifecycle,content reference or normalized host/user | exact hosts; no implicitwildcards/fetch; ordinary/personal lifecycles distinct; old examples excluded. |
| Platform: InboundReceipt/Cursor | physical update identity,logicalwork identity,source fence,admission/dedupe disposition,committed cursor/idle bootstrap state | no raw update; higher offset only after required durable acceptance; epoch/cursor reset cannot reopen business identity. |
| Platform: DurableWork | work/correlation IDs,scope,type,metadata-only parameters,admitted/due/expires_at,claim generation/owner/expiry,status | bounded slots; accepted work not evicted; original120s runnable clock; stale leases cannot authorize unknown replay. |
| Platform: Effect/Attempt/Receipt | stable parent/child effect IDs,exact target/action,policy/generation,fencedattempt,DISPATCHING time,result category,confirmed external IDs | intent durable before network; receipt is execution evidence only when trustworthy/exact; each chunk/member separate. |
| Platform: UnknownReview | effect ID,scope,typed reason,minimal evidence,owner,active/archive dates | scoped review;30d→archived UNRESOLVED_NO_REPLAY; no referral-as-retry/refund. |
| Observability: Audit/Evidence | scope/sequence,actor/action/target,result/correlation,previous digest,event digest,key version | allowlisted noncontent; append-only attributed outcomes; independent signed checkpoint detects tail changes. |
| Observability: Alert | detectable threshold,produced_at,owner/runbook,severity,delivery category/evidence | production<=300s; CONFIRMED/UNKNOWN/NOT_DELIVERED distinct; all-sink loss not PASS. |
| Operations: ImportManifest/Disposition | source+mapping revision,snapshot watermark/checksum,synthetic surrogate row IDs,exact disposition,count/total/report approvals | one disposition per candidate; no source secrets/history; real mappings in restricted approved evidence, not GitHub. |
| CONTENT: HistoryEntry | entry/owner/tenant/topic,question/answer role,encrypted body,capture consent/generation,original_at,effective_expiry,state | entry original+30d max; staged until metadata validates; owner-only; no backfill/expiry reset. |
| CONTENT: ContentDependency | child entry,all contributing owners/source entries,captured generations,source expiries | min expiry; source clear invalidates descendants; caller consent alone never transfers borrowed ownership. |
| CONTENT: ExampleBody | example/tenant ID,encrypted eligible submitted/synthetic text,provenance,original/effective expiry,approved version | explicit authored-personal permission or approved nonpersonal provenance; bounded count/bytes; no copied private member content. |

All scoped references use tenant-qualified keys/FKs within a store. Cross-store
references are validated through ports and idempotent publication; there is no
cross-database FK or atomic distributed transaction. Repository/RLS prevent accidental
cross-tenant reads/writes; global admin operations have distinct authorized ports.

## Transaction and cross-store protocols

Required audit/admission first commits before privileged evaluation; outcome and
metadata business mutation then share one application-owned unit of work.
READ COMMITTED with deterministic row locks+uniqueness is the baseline; use stronger
isolation only for a demonstrated invariant. Never SKIP LOCKED financial rows.
Deadlock/serialization retry is bounded pure DB work under same keys; uncertain
COMMIT is reconciled by identity, not assumed rolled back. No network inside transactions.

Grant: current verified group participation and bothswitches -> lockpolicy/wallet ->
observe unique current UTC week outcome -> GRANTED +1/event/balance OR SKIPPED_CAP0.
Only scheduler/explicitbalance triggers; no AI/Ads lazygrant/backfill.

Debit: trustworthy confirmed AD warning/update -> currentwarn/bothswitches/eligibility ->
lockwallet+unsetpaididentity -> atomic -1/event/DEBIT_CONFIRMED ifbalance>=1; else
appropriate terminal0. No repeat warning, delete charge, retro-charge or auto refund.
Already settled zero-charge or UNKNOWN outcomes cannot reopen when later evidence arrives.

Content publication: validate current METADATA consent/generations -> stage encrypted
CONTENT entry -> recheck all sources/current METADATA and publish reference idempotently.
Failure or concurrent clear makes entry unreachable; cleanup removes staged/orphaned
content. Each read/context/export/late answer checks current controls and min expiry.
Clear increments durable generations before erasure, suppressing late writes/effects;
metadata unavailable blocks content use. CONTENT failure cannot restore text from backup.


RequiredEvidence readiness requires synchronously durable METADATA plus independently
verified signed checkpoint coverage of the exact admission and relevant critical
control/state revision BEFORE privileged evaluation, success acknowledgment or ANY
dependent external effect. The witness covers actual critical-state/checkpoint
digests and acknowledgment boundaries, not merely an audit-chain head.

COMMITTED_UNWITNESSED is pending, never effective new allowance/grant authority,
consent permission, deletion activation, success or replay permission. Restrictive
denial/revocation/clear takes the safe effect immediately: block affected work,
never keep using stale permissive state while witness is unavailable. Reconciliation
uses canonical original identities; it does not infer rollback or nonexecution.
A witnessed manifest binds scoped commit/batch identity; relevant ledger/grant/paid/
effect outcomes; source/receiver fences; current policy/denial/revocation/consent/
clear/activation revisions; and audit-chain boundary. Recovery independently
reconciles actual canonical records against that manifest and durable copy.
Missing or conflicting proof blocks affected promotion; an audit-head signature
alone cannot certify wallet/control state. Cross-store publication, cursor
acknowledgment and all dependent dispatch require
this same barrier. A failed witness after a DB commit leaves recorded pending state,
not fabricated success or a repeated mutation.

## External state and claim transitions

PLANNED→READY→CLAIMED→DISPATCHING→APPLIED_CONFIRMED.
Before dispatch: invalid/revoked/expired/missing transient body→SAFE_TERMINAL.
Proved nonexecution→bounded same-identity retry; ambiguous afterDISPATCHING→UNKNOWN_HOLD.
UNKNOWN_HOLD→trusted read-only reconciliation outcome OR ARCHIVED_UNRESOLVED_NO_REPLAY
after30d. Reconciliation does not reopen a settled paid outcome or authorize replay.

A dispatch attempt is sync-durable before the network call. Lease expiry recovers
only work with no dispatch intent and a valid new fence; once DISPATCHING exists,
even crash-before-send is conservatively unknown. Stale workers cannot commit a
new attempt; a request already sent cannot be recalled by a database fence.
Partial chunks/member effects retain individual receipts and stop on ambiguity.

## Retention and recovery

D-013 [approved contract](recommended-contracts.md#d-013--collection-retention-backup-and-provider-processing)
is authoritative: content30d max/minsourceexpiry;ordinarymetadata30d;logs14d;
anonymousaggregates90d;audit365d;ledger365d with imported detail>=365d aftercutover;
compactcriticaloutcomes365d onlybehindprovedage/epoch/current week barriers;
unknownactive30dthenNO_REPLAY;staging7d;quarantinepayload30d/report365d;
encryptedmetadata-onlybackups<=30d. Ordinaryprofileclosure<=7d is not ledgerclear.

Detail purge needs independently reconciled NON_EVENT checkpoint/proven safety fences;
unresolved discrepancies never vanish by expiry. Critical consent/deny/revocation/
activation/clear/ledger/audit/outcomes RPO0 uses surviving verified sync state;
stale backup restoration blocks readiness until exact proof. Lost consentOFF.
No content-restoration promise. MVCC/WAL/key/storage erasure must be proved separately;
SQL DELETE, encryption alone and “no backups” are not secure-erasure evidence.
