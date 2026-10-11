# Feature Specification: Global Rewrite Behavioral Baseline

**Feature Branch**: `main`

**Created**: 2026-08-17

**Status**: Consolidated recommendation draft — awaiting whole-specification approval

**Last Reviewed**: 2026-10-11

**Input**: User description: "Rebuild `ai_bot6` as the globally deployable
`ai_bot_v2`, using the legacy repository only as a read-only behavioral reference.
Define and approve behavior, security, data migration, testing, and operational
expectations before architecture planning or implementation."

GitHub `vahid162/ai_bot_v2` is the authoritative source and the only write target
for the current work. No server files, checkout, containers, services, or data,
including `/www/dk_project/dk_app/guardbot`, may be changed or synchronized.
Server synchronization or deployment requires new explicit project-owner
authorization; if later authorized, it must consume a reviewed GitHub commit.
Publication of this draft does not approve its scope or authorize implementation.
The branch-level evidence ledger, supplementary cases, synthetic corpus seed and
proposed measurement contracts are linked below. Static discovery and case
pointers do not prove exhaustive coverage, approved scope or passing evaluation;
working policy contracts and finite reference bounds are now selected in
[recommended-contracts.md](recommended-contracts.md). Actual corpus/gold labels,
external agreements, actual import mappings, measured results and whole-specification
approval remain distinct gates; see [review-summary.md](review-summary.md).

## Clarifications

### Session 2026-10-11

- Q: Should the first cutover migrate legacy conversation history or start the new
  version with empty history? → A: Start the new version with empty conversation
  history. This does not authorize deleting legacy data or migrating other entity
  types.
- Q: When a user's tokens are exhausted, should AdsGuard respect the group's
  warning or no-action mode, or delete the message as in the legacy version?
  → A: Respect the group setting; delete only in deletion mode. Token exhaustion
  does not authorize deletion in warning or no-action mode, and existing
  authorization, exemption, and classification safeguards still apply.
- Q: If a disconnect leaves the result of a Telegram reply, warning, or deletion
  unknown, should the system retry automatically or check the result and escalate?
  → A: Check the result first using trustworthy evidence actually available. If
  it remains unknown, record it for operator review without automatic retry.
  An unresolved result is not confirmed success or confirmed non-execution;
  referral alone does not authorize manual repetition.
- Q: How should new conversation text be stored and retained? → A: Store user
  questions and assistant answers only after the owner explicitly opts in with
  policy notice; automatically delete each message 30 days after its original
  creation. The owner can clear eligible history earlier or export it. This
  decision does not set token-ledger or audit-record retention.
- Q: What safety criterion permits automatic AdsGuard deletion? → A: Zero false
  deletions in an independently labelled, project-owner-approved test corpus,
  followed by separate explicit project-owner approval to activate the tested
  deletion policy. Automatic deletion remains disabled until both conditions are
  met. Zero observed test errors is not a guarantee of zero real-world errors;
  approving this requirement is not approval to activate deletion.

- Q: Should automatic deletion of forbidden forwards and captionless media also
  respect the group's AdsGuard mode? → A: Yes (Option A): respect the group mode;
  automatic user-content deletion is permitted only in deletion mode, with current
  eligibility, authorization, exemptions, the FR-027 quality gate and separate
  project-owner activation approval. Disabled, no-action and warning states do not
  permit these guards to delete user content. Profiles and queued/recovered work
  cannot bypass the current effective mode. This approves only D-007's mode rule,
  not caption/grace defaults, manual mute-associated deletion, ordinary chat cleanup,
  corpus labels/evaluation, activation, the whole specification or the later plan.

- Q: Should /block explicitly deny private conversation even when global access
  is enabled or another allowance exists, or merely remove one allowance? → A:
  Explicit denial (Option A): /block denies private conversation access regardless
  of ordinary global, environment or managed allowances until a currently
  authorized superadmin explicitly unblocks the user. An ordinary allow action or
  policy toggle does not clear the denial; unblocking does not itself grant access
  or bypass other conversation gates. This is D-006's access policy only, not
  Telegram user blocking, group moderation, role revocation, legacy-state import,
  approval of command naming, the whole specification, plan or implementation.

- Q: When AdsGuard is in no-action mode and trusted eligible content is classified
  as advertising, should the user's tokens be spent? → A: No (Option A): AdsGuard
  in no-action mode consumes no tokens, including with sufficient balance. This
  applies to singles/albums, repeated or edited inputs and queued/recovered
  processing; it must not produce a debit or successful-spend event for that
  no-action journey. This approves only D-008's no-action charging branch, not
  warning/deletion charges, numeric grant/carry/spend rules, legacy corrections,
  automatic refunds, the whole specification, plan or implementation.

- Q: Should the first cutover preserve validated legacy token balances and
  associated token records or start new wallets without the old balance? → A:
  Preserve them (Option A), after separate approval of actual sanitized schema
  mappings and exact tenant/user reconciliation. Include associated ledger records
  and only necessary validated grant/deduplication evidence to prevent a second
  grant or debit. Missing/conflicting evidence is rejected or quarantined, not
  guessed or repaired by fabricated events. This selects D-011's token-continuity
  intent only, not other entity imports, retention, migration execution, legacy
  deletion, whole-spec/plan approval or implementation.

- Owner instruction (2026-10-11): “بقیه ی موارد رو هم طبق Recommended پیش ببر”
  delegates selection of remaining recommendations for GitHub-only documentation.
  On that authority, remaining working D-006–D-013 contracts are now selected
  in recommended-contracts.md and integrated below. Previously selected D-009
  security normalization and D-012 Standard definitions remain unchanged. These are agent-selected recommendations under owner delegation, not
  answers to questions that were never asked. Actual schema/traffic/provider facts,
  independent corpus labels, corpus sign-off/results, deletion activation and
  complete specification/plan approval are not supplied by this instruction.

- Recommendation completion (2026-10-11): no new questions asked or invented
  answers recorded. Routes, moderation, wallet, bounds, import intentions and
  lifecycles are selected for consolidated review, not approved production facts.
  Whole-spec/plan/corpus/activation approval remains separate.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Approve a Traceable Rewrite Contract (Priority: P1)

As the project owner, I can review a complete inventory of legacy behavior and see
which behavior will be preserved, intentionally changed, or retired, so the rewrite
cannot silently lose capabilities or carry unsafe legacy choices forward.

**Why this priority**: Every later design and implementation decision depends on an
approved, evidence-backed scope.

**Independent Test**: Review the behavior classification table against the pinned
legacy source snapshot and confirm that every listed capability has evidence,
classification, acceptance coverage, and an owner decision.

**Acceptance Scenarios**:

1. **Given** a behavior observed in the legacy repository, **When** it is added to
   the rewrite inventory, **Then** it records its evidence, affected actor, proposed
   classification, data or security impact, and verification path.
2. **Given** a behavior proposed for change or retirement, **When** the project
   owner has not explicitly approved it, **Then** planning treats the behavior as
   unresolved and implementation remains blocked.
3. **Given** a legacy behavior discovered after this specification is approved,
   **When** it affects observable behavior or stored data, **Then** the
   specification is amended and re-approved before dependent work continues.
4. **Given** a legacy implementation detail with no user-visible contract,
   **When** it conflicts with the Constitution, **Then** it may be retired without
   preserving its internal structure, but the retirement remains traceable.

---

### User Story 2 - Continue Private and Group AI Conversations (Priority: P1)

As an authorized Telegram user, I can ask questions privately or in an enabled
group and receive a localized, context-aware response without seeing another user
or group's conversation.

**Why this priority**: AI conversation is the primary user value of the product.

**Independent Test**: Use isolated private and group accounts to exercise start,
help, direct questions, `/ask`, reply, mention, session, feedback, clear, and export
journeys while verifying tenant and user isolation. Include default-off storage,
explicit opt-in after notice, per-message 30-day expiry, early clearing, and
non-resurrection of content after recovery.

**Acceptance Scenarios**:

1. **Given** an authorized private user, **When** the user sends a supported
   question or uses `/ask`, **Then** the bot acknowledges the request, returns one
   localized answer, and associates the exchange with only that user's session.
2. **Given** an enabled group in mention mode, **When** a permitted member
   explicitly mentions or replies to the bot, **Then** the bot answers in the
   correct group and topic context.
3. **Given** an enabled group in all-message mode, **When** a permitted member
   sends an eligible message, **Then** the bot answers subject to the group's rate
   and permission policy.
4. **Given** a disabled group, an unaddressed mention-mode message, or a disallowed
   private user, **When** a message arrives, **Then** no AI request is made and any
   guidance shown is safe, localized, and rate-limited.
5. **Given** the answer provider times out or returns unusable output, **When** the
   request completes, **Then** the system records a safe outcome and provides
   localized guidance when delivery is safe, without duplicated messages, leaked
   provider data, or cross-session history. Unknown Telegram delivery follows
   scenario 7 rather than triggering a repeated reply or fallback notice.
6. **Given** a user requests history export or clearing, **When** ownership is
   verified, **Then** only that user's eligible retained, unexpired history in the
   selected scope is exported or cleared and the result is confirmed. At the first
   cutover this history starts empty; it remains empty until explicit opt-in.
   An empty history receives an explicit empty result rather than a backfilled
   export. Clearing makes its text unavailable to later history, export, or AI
   context; it does not delete token-ledger or audit records.
7. **Given** Telegram may have sent a reply before the connection or worker stops
   without recording completion, **When** recovery checks the available evidence,
   **Then** confirmed completion is not repeated, confirmed non-execution is
   eligible for normal authorized recovery, and a still-unknown result is recorded
   for operator review without automatically resending the reply or a fallback
   notice. A delivery attempt or deduplication record alone is not proof that the
   user received a reply.
8. **Given** a user has not explicitly opted in to history storage, **When** an
   eligible question and answer are processed, **Then** no conversation text or
   context-bearing reference is persisted as history; answering does not require
   history opt-in. The storage policy is explained before a choice, and neither a
   group administrator nor a default setting can opt in on the user's behalf.
9. **Given** the user explicitly opts in after policy notice, **When** a subsequent
   eligible exchange is created in the chosen owner and tenant scope, **Then**
   only new permitted questions and answers are retained with each message's
   original creation time and expiry at 30 days. Previously unconsented messages
   are not backfilled. The user can turn future storage off and separately clear
   existing eligible history or request an owner-scoped export.
10. **Given** retained user or assistant text reaches 30 days from its original
    creation, **When** expiry, context selection, or export is evaluated, **Then**
    the text is automatically deleted and is no longer available as history,
    export, or AI context. Reads, edits, exports, and repeated processing do not
    restart its retention clock; metadata-only audit and ledger records are not
    removed by this expiry.

11. **Given** a private-conversation user is explicitly blocked, **When** global
    access is on, environment/managed allowances exist, a private input route is
    invoked or previously admitted work later dispatches, **Then** no new provider
    call/private answer bypasses the current denial. Only a currently authorized
    superadmin's explicit unblock can remove it; a plain allow or global toggle
    cannot. Unblock still requires ordinary access/feature gates to pass, repeated
    block/unblock is idempotent/audited, and an already unknown Telegram result is
    held under FR-064. No group-message deletion, role revocation or history
    erasure follows from this private-conversation policy.

---

### User Story 3 - Govern Groups with Verified Administration (Priority: P1)

As a verified group administrator or superadmin, I can manage only the groups and
global controls I am authorized for, from group commands or a private management
panel, with every privileged change auditable.

**Why this priority**: The bot moderates groups and changes consequential policy;
authorization and tenant isolation are release-critical.

**Independent Test**: Exercise the same management actions as a group member,
group administrator, anonymous administrator, removed administrator, and
superadmin across two unrelated groups.

**Acceptance Scenarios**:

1. **Given** a current group administrator, **When** the administrator opens the
   private panel through the supported management journey, **Then** only manageable
   groups are offered and the chosen active-group context is explicit.
2. **Given** two administrators using private panels concurrently, **When** each
   changes a group setting, **Then** each change applies only to that
   administrator's selected group.
3. **Given** a user who is not currently authorized, **When** the user attempts a
   privileged command or callback, **Then** the action is denied without exposing
   configuration and the denial is auditable.
4. **Given** a superadmin in an approved private context, **When** global direct
   message policy, administrator membership, logging controls, or global feature
   switches are changed, **Then** the change is validated, recorded, and takes
   effect according to the configuration contract.
5. **Given** an administrator loses group privileges after opening the panel,
   **When** a later callback is submitted, **Then** authorization is re-evaluated
   and stale authority is not accepted.

---

### User Story 4 - Apply Explainable AdsGuard Policy (Priority: P1)

As a group administrator, I can configure advertising protection and safely apply
no-action, warning, or deletion policy to eligible group content while protecting
approved users, domains, replies, and multi-message albums from incorrect or
duplicate enforcement.

**Why this priority**: AdsGuard is a core moderation capability with destructive
actions and therefore needs precise, independently verifiable behavior.

**Independent Test**: Run an independently labelled, project-owner-approved
corpus of eligible advertisements, legitimate non-exempt content, text, captions,
replies, forwards, contact-like content, allowlisted content, and albums through
each policy mode, including provider failure, exhausted wallets, repeated
delivery, and pending-action recovery. Evaluate proposed delete decisions against
the approved labels independently of the disabled activation gate: no legitimate
item may be proposed for deletion, and advertisement cases must pass their
expected functional scenarios. Record the tested policy and corpus revisions;
simulation does not execute moderation actions or spend tokens.

**Acceptance Scenarios**:

1. **Given** AdsGuard is enabled with a configured threshold, **When** eligible
   content is classified above that threshold, **Then** the configured action
   follows current authorization, exemptions, and the recorded policy. Automatic
   deletion is scheduled or applied only if the FR-027 quality gate and separate
   project-owner activation approval are valid; otherwise deletion is blocked
   with a traceable reason. A confidence threshold alone is not deletion approval.
2. **Given** content from an exempt administrator or allowlisted user, or content
   matching an approved domain or reply exemption, **When** it is evaluated,
   **Then** no destructive moderation action occurs and the exemption reason is
   traceable.
3. **Given** a media album arrives as several updates, **When** it is classified
   and acted upon, **Then** it is treated as one logical item with at most one
   warning, one charge, and one final policy outcome.
4. **Given** a delayed action worker stops before recording the result of a
   warning or deletion, **When** another worker checks trustworthy available
   evidence, **Then** confirmed completion is not repeated, confirmed
   non-execution permits normal bounded recovery with current authorization and
   policy checks, and a still-unknown result is held for operator review without
   automatic retry. This applies to single messages and each affected album item;
   a partially completed album is not replayed as a whole, and no second token
   charge occurs.
5. **Given** an administrator runs probe, statistics, or threshold simulation,
   **When** results are displayed, **Then** no real moderation action or token
   spend occurs.
6. **Given** classification is unavailable, malformed, or below the approved
   confidence threshold, **When** policy is evaluated, **Then** the system does
   not perform a destructive action and records an operator-visible reason.
7. **Given** eligible advertising and an exhausted wallet in warning mode,
   **When** the configured policy is applied to a single message or album,
   **Then** only the configured warning is applied, no message is deleted, and
   repeated delivery produces no duplicate warning or token charge.
8. **Given** eligible advertising in no-action mode with sufficient or exhausted
   balance and any token-switch combination, **When** AdsGuard evaluates a single
   message or album, including duplicate/edit or queued/recovered processing,
   **Then** no moderation action occurs and this AdsGuard journey creates zero
   token debit or successful-spend events. Repeated processing cannot turn that
   recorded no-charge outcome into a spend; unrelated authorized grants remain
   separate. The delegated D-008 working contract additionally charges only a
   confirmed eligible warning, never delete, and never reopens this identity.
9. **Given** an exhausted wallet in deletion mode, **When** a single message or
   album is evaluated, **Then** deletion is permitted only if the configured
   policy, current authorization, exemptions, the FR-027 quality gate, and separate
   project-owner activation approval allow it; exhaustion alone is not a deletion
   trigger, and repeated delivery must not duplicate the action or charge.
10. **Given** deletion mode is selected but the approved corpus, successful
    zero-false-deletion evaluation, or separate project-owner activation approval
    is missing or no longer valid for the tested scope, **When** a new, queued, or
    recovered deletion is considered, **Then** no automatic user-content deletion
    is attempted, regardless of wallet balance or provider confidence, and the
    blocked reason is recorded. An unknown external result still follows FR-064;
    activation approval does not authorize a repeat of an unresolved action.

11. **Given** an automatic forbidden-forward or captionless-media guard for a
    single message or album, **When** its action is evaluated under disabled/off,
    no-action, warning or deletion policy, **Then** it respects the current group
    mode: no automatic user-content deletion outside deletion mode, and no-action
    performs no moderation action. A deletion-mode candidate is permitted only
    with current eligibility, authorization, exemptions, FR-027 quality evidence
    and separate owner activation approval. Profiles or pending work scheduled
    before a mode change cannot override the mode at execution/recovery; an unknown
    prior external result remains held under FR-064. Exact grace/cancel boundaries,
    supported origins and warning output remain unapproved, not assumed.

---

### User Story 5 - Migrate and Cut Over without Data Corruption (Priority: P1)

As the project owner and operator, I can rehearse migration from legacy data,
account for every accepted or rejected record, cut over within an approved window,
and recover safely without importing secrets or losing tenant boundaries.

**Why this priority**: The observed legacy schema and runtime contracts diverge in
several areas, so a direct copy would risk silent data loss and authorization
errors.

**Independent Test**: Migrate a sanitized, production-like legacy snapshot that
contains valid rows, duplicate rows, schema variants, orphaned rows, invalid tenant
references, and credential-like values; then compare reconciliation results and
exercise rollback.

**Acceptance Scenarios**:

1. **Given** a legacy snapshot, **When** a migration rehearsal runs, **Then** every
   source record is classified as imported, transformed, rejected, quarantined, or
   intentionally excluded with a reason.
2. **Given** legacy session, history, administration, AdsGuard, or ledger records
   whose stored shape conflicts with runtime assumptions, **When** they are
   inspected, **Then** legacy conversation content and context-bearing references
   are intentionally excluded, and only separately approved non-history entities
   are transformed through an approved mapping without guessing a tenant or owner.
3. **Given** a credential or credential-like value in legacy source or data,
   **When** migration runs, **Then** it is excluded, reported without revealing its
   value, and replaced only by a separately rotated deployment secret.
4. **Given** migration validation fails or reconciliation exceeds an approved
   threshold, **When** cutover is evaluated, **Then** release is aborted and the
   prior service and data state remain recoverable.
5. **Given** a successful cutover with separately approved actual token mappings,
   **When** post-cutover checks run, **Then** validated legacy wallet balances and
   associated ledger records preserve their approved tenant/user identities and
   reconcile exactly to approved evidence. Necessary validated grant/deduplication
   state prevents a second grant or debit; its mere existence cannot turn a failed
   spend into success. Missing/conflicting evidence blocks promotion/cutover rather
   than creating guessed balances or synthetic balancing events. Other entities
   reconcile only if separately selected and mapped; their import is not approved
   by token continuity. Conversation history and AI context start empty.
6. **Given** a sanitized legacy fixture containing conversation messages and
   context-bearing session or provider references, **When** first-cutover exclusion
   is verified, **Then** no legacy conversation appears in new-bot history, export,
   or conversational AI context; exclusion counts record the reason, and legacy source data
   remains unchanged.

---

### User Story 6 - Receive and Spend Weekly Tokens Reliably (Priority: P2)

As a user in a token-enabled group, I can see my group-specific balance, receive at
most one weekly grant up to the configured carry limit, and spend at most one token
for one eligible logical moderation journey.

**Why this priority**: Tokens support an important AdsGuard interaction, but the
core chat and moderation journeys remain useful without the feature enabled.

**Independent Test**: Run wallet display, lazy and scheduled weekly grants,
single-message and album spending, duplicate updates, insufficient balance, and
concurrent requests for the same user in two groups.

**Acceptance Scenarios**:

1. **Given** the weekly grant is due, **When** both scheduled and user-triggered
   grant checks occur concurrently, **Then** the user receives at most one grant
   for that tenant and week.
2. **Given** the wallet is at its carry limit, **When** a weekly grant is checked,
   **Then** the balance does not exceed the configured limit.
3. **Given** an eligible logical action is delivered repeatedly, **When** spending
   is requested, **Then** the balance decreases once and repeat attempts return the
   same outcome without a second charge.
4. **Given** insufficient balance or a disabled token feature, **When** spending is
   requested, **Then** no token is charged, no negative balance is possible, and
   localized guidance is confined to an explicit balance/eligible warning journey,
   never unsolicited off/none moderation. An insufficient balance does not change
   the effective AdsGuard action mode: warning remains warning, no-action remains
   no-action, and deletion still requires the normal policy safeguards.
5. **Given** the same user participates in two groups, **When** balances are
   viewed, **Then** each tenant's wallet, grant, and ledger remain independent.

6. **Given** AdsGuard's effective mode is no-action before a not-yet-committed
   charge, **When** trusted eligible advertising is processed for a single item
   or album with any balance, **Then** no token is spent and no successful-spend
   ledger event is recorded. Profiles, stale admission, duplicate/edit processing,
   restart and recovery cannot bypass the no-charge rule. Independent authorized
   grants may still change the wallet; this decision neither refunds an already
   committed/historical charge. Delegated D-008 additionally selects confirmed
   eligible warning-only charging, distinct from this historical answer.

---

### User Story 7 - Use a Consistent Global and Localized Experience (Priority: P2)

As a user or administrator, I receive complete, consistent messages in the selected
language, while group settings override global defaults without leaking into other
groups.

**Why this priority**: Global deployment requires predictable configuration and
complete language behavior rather than environment-specific forks.

**Independent Test**: Repeat core user and administrator journeys in Persian,
English, Arabic, Turkish, and Russian across groups with different settings and
verify fallback, directionality, formatting, and isolation.

**Acceptance Scenarios**:

1. **Given** a group language is selected, **When** a supported journey runs,
   **Then** every user-visible message and button uses a complete catalog for that
   language with correct direction and formatting.
2. **Given** a group-specific value, a global value, a deployment-provided value,
   and a safe default all exist, **When** configuration is resolved, **Then** the
   documented precedence produces one predictable value.
3. **Given** a configuration update is invalid, **When** an administrator submits
   it, **Then** the value is rejected, the prior effective value remains, and the
   result is auditable.
4. **Given** a translation entry is missing or malformed, **When** release
   readiness is checked, **Then** the release is blocked rather than exposing a raw
   key or developer-only message.

---

### User Story 8 - Operate and Recover the Service Safely (Priority: P2)

As an operator, I can determine whether the bot and its critical dependencies are
ready, understand failures without seeing private content, and recover interrupted
jobs or releases without duplicate user-visible effects or host-wide changes.

**Why this priority**: Reliable global operation depends on safe diagnostics,
isolated deployment, and recoverable background work.

**Independent Test**: Exercise startup, shutdown, dependency outage, provider
timeout, worker interruption before and after an external effect, lost
acknowledgments, duplicate updates, operator-review holds, backup restore, release,
and rollback under the approved production-equivalent security boundary.

**Acceptance Scenarios**:

1. **Given** a critical dependency is unavailable, **When** readiness is checked,
   **Then** the service reports not ready without leaking credentials or private
   messages while unrelated host services remain unaffected.
2. **Given** an update or job is recovered after interruption, **When** its
   external result is checked, **Then** available trustworthy evidence determines
   confirmed completion, confirmed non-execution, or an unresolved operator-review
   state. Unknown replies, warnings, or deletions are not automatically retried;
   restarts, repeated updates, and abandoned-claim recovery preserve that hold.
   Correlation and deduplication remain traceable but do not prove external
   execution or authorize another token charge.
3. **Given** a release or rollback, **When** the operator follows the approved
   procedure, **Then** only project-scoped resources change and no global host tool
   installation is required.
4. **Given** a security, availability, queue-age, or reconciliation threshold is
   breached, **When** monitoring evaluates it, **Then** an actionable, redacted
   alert points to an owned recovery procedure.
5. **Given** a backup contains conversation text that has since expired or been
   cleared, **When** restoration or recovery is rehearsed, **Then** the text is
   excluded or deleted before history, export, or AI context becomes available;
   recovery does not resurrect the conversation or erase separately governed
   token-ledger and audit records.

### Edge Cases

- Telegram delivers the same update more than once, out of order, or after a
  callback has expired.
- A group is upgraded, renamed, migrated to a new identifier, deleted, or removes
  the bot while an administrator still has an active private-panel context.
- A user opens multiple panel messages or switches the active group while a prior
  force-reply or callback remains outstanding, or a private-conversation user is
  blocked after admission but before delayed provider/answer dispatch. Current
  scope/access is re-checked; stale acceptance is not authority.
- A group administrator becomes a regular member between authorization display and
  action submission, or sends as an anonymous administrator or channel.
- A forwarded message, automatic channel forward, edited message, service message,
  empty caption, very long message, unsupported media, or malformed entity arrives.
- An album arrives partially, late, with an edited caption, or with one item already
  deleted.
- The answer or classification provider times out, returns invalid structured
  output, embeds an unsafe URL, repeats a response, or returns content for a
  different request.
- A configuration value changes while requests or pending moderation actions are
  already in flight, including a queued forward/caption deletion whose group is
  disabled or switches to warning/no-action before execution. The current mode
  blocks that deletion; an unknown prior result remains held, not retried.
- Weekly grant, lazy grant, token spend, and duplicate cleanup run concurrently
  across multiple workers, or a wallet remains exhausted when AdsGuard evaluates
  warning, no-action, or deletion policy for a single message or album.
- A reply, warning, or deletion may reach Telegram before the connection or worker
  stops without recording completion, including a partly completed album. Recovery
  checks available evidence and holds unresolved items for operator review without
  automatic retry, including after another restart or repeated update.
- A language catalog is unavailable, contains a formatting mismatch, or receives an
  unsupported locale tag.
- A legacy export contains duplicate identities, orphaned rows, impossible
  timestamps, invalid encodings, negative balances, conflicting tenant ownership,
  or schema variants not present in the approved mapping.
- A backup is incomplete, stale, encrypted with an unavailable key, or cannot be
  restored within the approved cutover window, or contains conversation text that
  has since been cleared or reached its 30-day expiry during an in-flight request,
  export, or recovery. Access and recovery must not resurrect that text.
- Secret scanning detects a credential-like value in legacy data, documentation,
  build input, logs, or release artifacts.
- Audit, metrics, or error-reporting sinks are unavailable or apply backpressure.
- The service reaches a configured rate, concurrency, queue, or provider quota.

## Requirements *(mandatory)*

### Behavioral Scope and Classification

The following classification is the proposed rewrite baseline. `Preserve` means
retain the approved externally observable capability, not every unsafe legacy
branch or the old internal design. The whole proposed scope, including listed
Changes and Retirements, requires explicit owner approval. The five earlier
answers, D-006's explicit private-conversation deny rule, the D-007 automatic
forward/caption mode rule and D-008's no-action/no-charge rule are approved within
their stated limits. Remaining interaction/import, content/grace/manual-action
and other token-policy choices are not. Owner delegation selects D-009's stated
security normalization and D-012's Standard measurement definitions, not the
remaining contracts/evidence or the whole scope. D-011 now selects validated wallet/ledger
continuity only, conditional on separately approved actual mappings and exact
reconciliation; other entity imports remain undecided. Mixed records expose
proposed security/product differences rather than silently treating
them as Preserve; surfaced subbehavior decisions remain open in the decision
register. Exact source spans, impacts, owner status and case IDs are in the ledger.

| ID | Legacy capability or element | Proposed classification | Source evidence | Required outcome |
|----|------------------------------|-------------------------|-----------------|------------------|
| BR-001 | Start, help, identity, scoped command menus, and health journeys | Preserve discovery proposal; D-009 safe health normalization selected via delegation | [Exact evidence and case links](behavior-ledger.md#br-001--discovery-identity-menus-and-health) | Equivalent discoverability and safe responses by user scope |
| BR-002 | Private AI questions, `/ask`, reply input, typing state, and direct answers | Preserve input capability; Change route/context inconsistency (proposal) | [Exact evidence and case links](behavior-ledger.md#br-002--private-questions-ask-and-chunked-replies) | One isolated, localized outcome per accepted request |
| BR-003 | Group AI enablement, mention/all modes, administrator-only mode, throttling, and autocleanup | Preserve conversation modes; Change uniform fresh role/address gates; Retire hidden user cleanup (working recommendation; whole-scope approval pending) | [Exact evidence and case links](behavior-ledger.md#br-003--group-conversation-and-cleanup) | Per-group policy with no response outside eligible triggers |
| BR-004 | Conversation sessions, local history, clear, and export | Preserve capabilities; D-001/D-004; Change uniform reset/clear and transient borrowed exchange (working recommendation; whole-scope approval pending) | [Exact evidence and case links](behavior-ledger.md#br-004--sessions-history-clear-and-export) | Fresh initial AI context; default-off text storage, explicit owner opt-in after notice, per-message 30-day deletion, and owner-scoped export or earlier clear |
| BR-005 | Feedback, unknown-question capture, user reporting, and administrator review | Preserve feedback/report utility; Change uniform routes, scoped review and finite metadata (working recommendation; whole-scope approval pending) | [Exact evidence and case links](behavior-ledger.md#br-005--feedback-unknown-reporting-and-review) | Traceable feedback without cross-tenant disclosure |
| BR-006 | Global direct-message switch, allow/block policy, and user status listing | Preserve access controls; approved D-006 explicit deny precedence; D-009 current superadmin global-mutation security selected via delegation; UI/import details remain separately gated | [Exact evidence and case links](behavior-ledger.md#br-006--direct-message-access-and-user-listing) | Deny-by-default access with explicit, auditable exceptions |
| BR-007 | Private management panel, group binding, active-group selection, deep-link management, and force-reply editing | Preserve private management proposal; D-009 current authorization/original prompt target selected via delegation | [Exact evidence and case links](behavior-ledger.md#br-007--private-management-and-bound-prompts) | Fresh authorization and isolated administrator context |
| BR-008 | Group-admin and superadmin roles, superadmin membership, and privileged audit | Preserve scoped roles proposal; D-009 current authorization/redacted audit selected via delegation; outage contract open | [Exact evidence and case links](behavior-ledger.md#br-008--roles-superadmins-and-privileged-audit) | Least privilege, current-role verification, and complete audit |
| BR-009 | Per-group chat, AdsGuard, language, and command-menu controls | Preserve settings proposal; D-009 no false save/current callback scope selected via delegation | [Exact evidence and case links](behavior-ledger.md#br-009--scoped-settings-language-and-menus) | Validated settings with predictable scope and menus |
| BR-010 | Ads classification of text, captions, replies, forwards, contact-like content, domains, and media albums | Preserve classifier capability; Change strict score/eligibility/exemptions (working recommendation; whole-scope approval pending) | [Exact evidence and case links](behavior-ledger.md#br-010--adsguard-classification-and-eligibility) | One explainable decision per eligible logical item |
| BR-011 | AdsGuard off/none/warn/delete policy, threshold, cooldown, and cleanup behavior | D-002/D-005/D-007; Change structural-only deletion and presets; Retire hidden mute cleanup (working recommendation; whole-scope approval pending) | [Exact evidence and case links](behavior-ledger.md#br-011--modes-forwardcaption-guards-profiles-and-mute) | Exact per-group enforcement; exhaustion never promotes warning or no-action to deletion; automatic deletion requires zero false deletions in the approved test corpus and separate owner activation approval |
| BR-012 | Durable pending moderation, retries, stuck-work recovery, and album cleanup | Preserve durable work; D-003; Change fenced recovery and finite NO_REPLAY proof (working recommendation; whole-scope approval pending) | [Exact evidence and case links](behavior-ledger.md#br-012--pending-moderation-and-album-recovery) | Bounded safe recovery; reconcile external results first and hold unresolved actions for operator review without automatic retry |
| BR-013 | Ads examples, positive/negative labeling, user/domain allowlists, probe, statistics, simulation, and private debug control | Preserve scoped tools; Change exact aliases/provenance/lifecycles (working recommendation; whole-scope approval pending) | [Exact evidence and case links](behavior-ledger.md#br-013--examples-allowlists-and-diagnostics) | Tenant-scoped tools; diagnostics do not moderate content, change policy or spend tokens; ancillary effects declared |
| BR-014 | Tenant wallets, balance aliases, weekly grants, and carry limits | Preserve wallets; select +1 current-week grant/carry1, no implicit grant/backfill (working recommendation; whole-scope approval pending) | [Exact evidence and case links](behavior-ledger.md#br-014--wallet-balance-weekly-grants-and-carry) | One grant per tenant/user/week and accurate balance |
| BR-015 | Token spending, logical-message deduplication, and ledger history | Preserve ledger invariants; D-008; Change confirmed-warning-only charge and outcome/fence (working recommendation; whole-scope approval pending) | [Exact evidence and case links](behavior-ledger.md#br-015--token-spending-and-ledger-outcomes) | At most one approved charge; zero AdsGuard no-action debits; non-negative balance and reconcilable ledger |
| BR-016 | Database-first configuration with deployment fallback | Change proposal | [Exact evidence and case links](behavior-ledger.md#br-016--configuration-precedence-and-startup-seeding) | One uniform precedence chain with validation and safe defaults |
| BR-017 | Persian, English, Arabic, Turkish, and Russian selectors and fallback | Change proposal | [Exact evidence and case links](behavior-ledger.md#br-017--localization-coverage-and-fallback) | Preserve selectors while requiring complete catalogs and no raw-key fallback |
| BR-018 | Structured logging, metrics, health reporting, optional error reporting, provider warmup, and recurring cleanup | Preserve operations; Change mandatory evidence fail-closed and finite lifecycles (working recommendation; whole-scope approval pending) | [Exact evidence and case links](behavior-ledger.md#br-018--operations-health-and-recurring-work) | Redacted, actionable operations with idempotent jobs |
| BR-019 | Legacy operational data and schema history | Conditional selected per-entity import intentions; actual maps/reconciliation unapproved (working recommendation; whole-scope approval pending) | [Exact evidence and case links](behavior-ledger.md#br-019--candidate-imports-and-schema-drift) | Import only validated business data into one approved data contract |
| BR-020 | Schema upgrades during application startup and competing migration entry points | Retire competing ownership proposal | [Exact evidence and case links](behavior-ledger.md#br-020--migration-ownership) | One separately controlled, observable migration owner |
| BR-021 | Committed credentials, mutable release inputs, host-specific paths, fixed identifiers, and shared global deployment assumptions | Retire unsafe values/couplings proposal | [Exact evidence and case links](behavior-ledger.md#br-021--unsafe-and-nonportable-release-inputs) | Rotated secrets and isolated, portable project configuration |
| BR-022 | Large mixed-responsibility modules and direct coupling between transport, policy, provider calls, and data access | Retire internal coupling proposal | [Exact evidence and case links](behavior-ledger.md#br-022--mixed-internal-responsibilities) | Preserve outcomes without preserving unsafe internal structure |
| BR-023 | Tracked generated bytecode; other runtime artifacts not yet proved by discovery | Retire tracked bytecode proposal | [Exact evidence and case links](behavior-ledger.md#br-023--generated-artifacts-tracked-as-source) | Generated or local runtime artifacts never enter reviewed source |

Any newly discovered behavior is `Unclassified` by default and blocks dependent
planning until it is added to this table or a child specification with evidence and
an owner-approved decision.

The table is an inventory, not a completed traceability ledger. Before approval,
each entry needs an exact source location at the pinned revision, acceptance
scenario references, the deciding owner, and the impact/transition details required
by FR-005. Capability classifications remain proposals except the owner-approved
BR-004 first-cutover history exclusion and opt-in 30-day new-history policy, and
the BR-011 token-exhaustion mode-precedence Change recorded in Clarifications.
The owner also clarified unknown-result recovery for replies and BR-012 moderation
work and approved the BR-011 automatic-deletion quality and activation gate.
These clarified policies are not evidence of verified legacy differences and do
not approve the full capability inventory, activate deletion, or change BR-015's
grant, spending, or ledger invariants.

#### Evidence and Acceptance Package — 2026-10-11

The current documentation set supplements this specification without selecting an
architecture or implementing tests:

- [Behavior ledger](behavior-ledger.md): 23 indexed families with branch-level
  pinned source evidence, actors, proposed classifications, owner approval status,
  user/data/security impacts, transition communication and verification pointers.
  Source inspection is static; intended-but-broken calls and comments are not
  asserted as verified working behavior or evidence of the live database schema.
- [Additional acceptance cases](acceptance-cases.md): 93 supplementary
  Given/When/Then definitions (90 behavior cases plus three governance cases),
  with explicit pointers for all 65 FRs. A case whose oracle depends on an open
  owner choice is incomplete, not passing or ready to implement.
- [Synthetic corpus seed](test-corpus.md): 30 invented multilingual text/caption
  items, 12 structural variants and four history/import boundary fixtures. Labels
  are proposed, not independently labelled or owner-approved; visual content and
  complete representative coverage are not established. Candidate evaluation must
  exercise legitimate non-exempt negatives and eligible ad positives, not obtain
  a vacuous zero-false-deletion result by disabling the detector or gate. Logical
  albums and legitimate member effects are separately checked.
- [Measurement contracts](measurement-contract.md): denominators/clocks for all
  14 SCs and proposed substitute workload profiles, race samples, participant
  definitions and a maintenance window. No measured legacy peak or passing
  results are available. Distinguish interactive requests from passive moderation:
  no extra acknowledgment spam is authorized. That counting interpretation,
  D-012's Standard substitute, counting/clocks, sample sizes and drill-window
  definitions are now selected via owner-delegated recommendation; corpus coverage,
  independent gold labels and final bounded manifests remain separately reviewed.
  These selected definitions are not measurements or passing results.
- [Decision register](decision-register.md): historical explicit decisions remain
  scoped; all remaining working D-006–D-013 policy recommendations are selected
  under owner delegation in [recommended-contracts.md](recommended-contracts.md).
  [Review summary](review-summary.md) exposes tradeoffs and actual evidence gates.
  Whole-spec/plan approval, actual import maps/processor agreements, independent
  corpus sign-off, passing evaluation and deletion activation remain absent.


Source-level review surfaced differences requiring explicit disposition: legacy
block only revokes a database allow entry; group admins can reach global allow
mutations; some privileged reads/prompts/diagnostics lack equivalent fresh scope
checks; context/export/unknown-review paths differ by route; failed writes can
still appear successful; disallowed forwards and captionless-media guards can
schedule/delete before AdsGuard mode lookup; edited non-media input can exit
before classification; malformed scores can be treated as ads; failed token spend
can later appear successful solely from a dedupe row; dedupe expiry can allow an
old replay; and declared migration columns/keys differ from runtime assumptions.
These are pinned-source observations, not implemented fixes or confirmed live
failures. D-002 did not resolve forward/caption/mute/cleanup policy. The later
D-007 answer approves respecting the current group mode for automatic
forward/caption deletion; independent deletion outside delete mode is not allowed.
Remaining working eligibility/grace/mute/cleanup choices are selected in D-007
of recommended-contracts.md; they are not individual answers or deletion activation.
Every automatic AdsGuard user-content deletion remains subject to D-005.

Complete the remaining owner choices and approve corpus/workload definitions
before claiming specification readiness. Actual evaluations, corpus passing,
separate deletion activation and cutover rehearsals are later authorized gates;
this document work does not require or permit premature application code.

#### Verified Legacy AdsGuard / Token Interaction

Read-only review of
[`telegram_bot/ads_guard.py` at the pinned legacy commit](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2789-L2990)
identified these branches relevant to BR-011 and BR-015:

| Legacy condition after advertising classification | Observed legacy behavior |
|--------------------------------------------------|--------------------------|
| Effective token switches enabled; action is not `delete`; sufficient balance | Attempts one idempotent token spend for the logical message or album, then continues the ordinary moderation policy |
| Effective token switches enabled; action is not `delete`; insufficient balance | Attempts deletion, including album handling, sends an exhaustion-specific notice, and stops ordinary policy processing |
| Action is `delete` | Skips the token-spend branch and continues the deletion policy |
| Effective token switches disabled | Skips spending and continues the ordinary moderation policy |

The table remains historical source evidence. The owner-approved BR-011 `Change`
removes the legacy insufficient-balance deletion override: warning mode warns
only, no-action mode performs no moderation action, and deletion is permitted
only in deletion mode with the existing authorization, exemption, and approved
classification-quality safeguards. US4 scenarios 7–9, US6 scenario 4, FR-024,
FR-028, FR-040, and SC-005 verify this difference for single messages, albums, and
repeated delivery. Group administrators and affected users must be told that
exhaustion no longer causes implicit deletion before this change is activated;
legacy data is not modified and wallet integrity controls remain unchanged.
This clarification does not approve unsolicited exhaustion notices, mute
escalation, the other observed token branches, or the full specification.

The later D-008 answer intentionally changes the sufficient-balance no-action
branch as well: AdsGuard no-action consumes no tokens, regardless of balance,
token switches, single/album shape or repeated processing. FR-039/040, US4.8,
US6.6 and SC-005/006 verify this scoped difference. Tell group administrators and
affected users before later authorized activation; no legacy debit is rewritten
or refunded by this answer. Subsequent delegated D-008 recommendations select
warning-only confirmed charging and numerical/grant/replay/correction contracts.
The source table above stays unchanged historical evidence, not the rewrite policy.


### Functional Requirements

#### Scope, Evidence, and Approval

- **FR-001**: The rewrite MUST treat `ai_bot6` as an immutable, read-only reference
  and MUST NOT modify its files, history, branches, issues, pull requests,
  configuration, or deployments.
- **FR-002**: Every discovery record MUST identify the exact legacy revision and
  evidence location from which the behavior was inferred.
- **FR-003**: Every externally observable legacy behavior MUST be classified as
  preserve, change, or retire and linked to at least one acceptance scenario before
  dependent planning.
- **FR-004**: Unclassified observable behavior MUST default to unresolved rather
  than being silently changed or omitted.
- **FR-005**: Every proposed change or retirement MUST state user impact, data
  impact, security impact, transition communication, and verification method.
- **FR-006**: Implementation MUST remain blocked until the project owner explicitly
  approves this specification and the later architecture and migration plan.

#### Conversation and User Experience

- **FR-007**: Authorized users MUST be able to discover applicable commands and
  obtain start, help, identity, and safe service-status information appropriate to
  private, group, administrator, or superadmin scope.
- **FR-008**: Private-message access MUST follow the approved global policy and
  explicit allow/block decisions, with denial that reveals no protected data.
  An explicit private-conversation block MUST override ordinary global-on,
  environment and managed-user allowances on every private conversation route,
  including /ask and replies. Only a currently authorized superadmin's explicit
  unblock action may clear that denial; adding an allowance, toggling global
  access or restarting MUST NOT silently clear it. Without an explicit denial,
  normal approved access and feature gates still apply; unblocking is not itself
  an allowance. Not-yet-executed queued/recovered private provider work and answer
  delivery MUST re-check current access before dispatch; stale admission cannot
  bypass a later block. Unknown earlier Telegram effects still obey FR-064 and
  MUST NOT be automatically repeated. This rule does not block Telegram users at
  the platform level, moderate group messages, remove roles or erase history;
  management/self-service actions retain their independent authorization rules.
- **FR-009**: Authorized users MUST be able to submit questions through ordinary
  eligible messages, `/ask`, and supported reply journeys. Each accepted request
  MUST have one correlated outcome: a confirmed reply, a safe failure outcome, or
  an unresolved operator-review state under FR-064. An unresolved delivery MUST
  NOT be reported as a received reply.
  All plain-private, /ask arguments, reply text/caption and bound prompts use one pipeline; arguments take precedence, empty /ask opens a five-minute prompt without provider work. Exact aliases/routes and deliberate repairs are D-010 in recommended-contracts.md, not assumed working source branches.

- **FR-010**: Each group MUST independently support disabled, explicit-address, and
  all-eligible-message conversation behavior, plus an optional
  administrators-only restriction and bounded response frequency.
  Current off/address/all/admin-only gates apply to EVERY route including /ask; exact addressing/topic/fresh role evidence, no anonymous-ID or forward-origin bypass. Original-user AI/command cleanup is retired initially; only positively identified bot-owned cleanup is eligible.

- **FR-011**: Conversation context MUST be isolated by tenant, user, and applicable
  topic or session so that content cannot cross those boundaries. Retained history
  used as context MUST satisfy FR-058's owner opt-in, scope, and expiry rules;
  cleared or expired text and context-bearing references MUST NOT restore it.
  Without history opt-in, processing the current exchange MUST NOT create durable
  conversation history.
  Without opt-in use current exchange only; context <=20 own live entries/16,000 Unicode scalars. An exchange borrowing others' text including derived answer stays transient unless EVERY contributing owner has valid scope/consent/eligible source. /start resets context/home, not history/consent/ledger; /clear removes own retained history and resets context. Prior generations invalidate so late results cannot restore erased text.

- **FR-012**: Users MUST be able to clear and export only the conversation history
  they own and are permitted to access, with an explicit result. Exports MUST
  include only eligible retained, unexpired text; empty history MUST produce an
  explicit empty result. Users MUST be able to clear history before its automatic
  30-day expiry and turn off future text storage. Clearing MUST NOT erase
  token-ledger or audit records. For the new deployment, history starts empty at
  the first cutover; legacy history MUST NOT appear through these journeys.
  Group export is private-only to verified owner; failed/unavailable/unknown delivery never falls back to public transcript or false success. Conversation denial does not remove independently authorized self-service. Anonymous principals own no personal history.

- **FR-013**: Accepted requests MUST provide timely progress indication without
  producing unbounded or orphaned activity indicators.
- **FR-014**: Provider timeout, invalid output, rate limit, or outage MUST result in
  a safe localized outcome and MUST NOT duplicate replies or expose provider
  internals. If Telegram delivery is unknown, FR-064 applies: neither a repeated
  reply nor a fallback notice may be sent automatically to resolve the ambiguity.
- **FR-015**: Users MUST be able to record positive or negative feedback and report
  an unknown answer; administrators MUST be able to review only unknown items
  within their authorized scope. Feedback or unknown-item records MUST NOT bypass
  FR-058 by retaining unconsented, cleared, or expired conversation text; eligible
  non-content metadata remains distinct from raw conversation content.
  Feedback only for confirmed logical answers: private owner or one immutable vote per currently verified named group member. Unknown reports owner-only; bind original answer/tenant/topic/actor, identical repeats idempotent, conflicting second vote rejected. Final confirmed chunk alone carries controls.

#### Identity, Administration, and Tenancy

- **FR-016**: The product MUST distinguish regular users, group administrators,
  anonymous group administrators, and superadmins with deny-by-default privileges.
  Anonymous authority needs current exact-group platform evidence, not special user ID/forward origin; group-only ephemeral behavior, no named wallet/history/vote/global privilege. Quota principals tenant-bound.

- **FR-017**: Authorization MUST be re-evaluated against current scope for every
  privileged command, callback, force-reply, and delayed action. A force-reply or
  callback MUST remain bound to its original actor, target tenant and operation;
  switching active group MUST NOT silently retarget an outstanding interaction.
  Revoked, expired or mismatched authority MUST deny it without cross-scope reads
  or writes. Exact topic/generation/prompt binding and expiry are now selected
  in D-010/D-012 of recommended-contracts.md.
- **FR-018**: A verified administrator MUST be able to open a private group picker,
  select an active manageable group, and see which group a subsequent action will
  affect.
- **FR-019**: Active-group and panel state MUST be isolated per administrator and
  MUST expire or fail safely when stale, malformed, or no longer authorized.
  Bind actor/tenant/topic/operation/exact prompt/answer ID/generation; prompts-confirmations five-minute absolute, panels15-minute idle/one-hour absolute. Expiry prevents new submission, not safe disposition of admitted work; stale replies never fall through to AI.

- **FR-020**: Group administrators MUST be able to view and change the approved
  chat, AdsGuard, token, language, and cleanup settings for their own group.
- **FR-021**: Superadmins MUST be able to view and manage global direct-message
  policy, explicit user access, superadmin membership, global feature switches, and
  approved diagnostic controls only from an authorized context. Explicit private
  conversation block/unblock changes MUST be currently superadmin-authorized,
  idempotent and audited; group-admin status alone MUST NOT grant global authority.
  The selected NEW route is a private current-superadmin DM-policy panel,
  numeric user ID, explicit-denial unblock, and bound confirmation. No existing
  /unblock command is claimed; this clears denial only, granting no allowance.
- **FR-022**: Command menus and management entry points MUST reflect the actor's
  current scope without granting authority by visibility alone.
- **FR-023**: Every privileged attempt, whether accepted or denied, MUST produce a
  redacted audit event with actor, target scope, action, result, reason, time, and
  correlation identifier.
  Durable redacted attempt evidence precedes privileged processing. Mandatory audit/business-outcome failure blocks affected admission/dispatch; no silent swallow/fake recorded denial/success. Raw arrival is not admission. Required redacted correlation/telemetry production remains mandatory during optional exporter outage; no new Telegram notice without safe outcome tracking. D-009 defines in-flight uncertainty.

#### AdsGuard

- **FR-024**: Each group MUST have independent AdsGuard enablement, action mode,
  confidence threshold, request frequency, training-example limit, and cleanup
  policy. Insufficient token balance MUST NOT override the effective action mode:
  warning permits only a warning, no-action permits no moderation action, and
  deletion is permitted only in deletion mode subject to the normal safeguards,
  FR-027's quality gate, and separate project-owner activation approval. Group
  mode selection or a token balance MUST NOT bypass that activation gate.
  Automatic forbidden-forward and captionless-media guards MUST also respect the
  current effective group mode: disabled/off and no-action produce no moderation
  action, warning MUST NOT delete user content, and deletion mode permits deletion
  only when current eligibility, authorization, exemptions and FR-027's quality
  and separate owner-activation gates allow it. Guard/profile switches MUST NOT
  independently authorize deletion in another mode. Queued and recovered guard
  actions MUST re-check the effective mode at execution; scheduling under a prior
  deletion mode is not permission after the mode or enablement changes. The
  remaining warning, caption/grace, mute and cleanup working choices are now
  selected under D-007; no choice here activates deletion.
  Defaults: Ads OFF, enabling-without-action none; AD finite numeric [0,1] score>=0.90; gap 2s (0–3600), few-shots 10 (0–50). Presets preview exact changes: free none/guards OFF, medium warn/caption ON, strict warn/both guards ON; preserve other valid values, never activate deletion.

- **FR-025**: AdsGuard MUST evaluate all approved eligible content forms and MUST
  apply administrator, user, domain, reply, contact-like, and other approved
  exemptions before any destructive action.
  Attributable text/captions including edits/manualforwards, not binary/OCR/transcription/URL fetch. Unknown role/scope/exemption and invalid scores cannot destroy/debit. D-007 defines exact all-host domain and short-reply exemptions. Structural forward/caption violation ALONE NEVER deletes: independently qualifying AD plus all current delete safeguards required.

- **FR-026**: Every decision MUST record the logical content identity, tenant,
  policy revision, classification outcome, confidence when available, exemption or
  action reason, and final status without retaining unnecessary message content.
  Strict AD/NOT_AD plus finite numeric score; malformed/boolean/string/missing becomes UNKNOWN. Final outcome, raw content and external execution proof are distinct.

- **FR-027**: Missing, malformed, untrusted, timed-out, or insufficient-confidence
  classification MUST NOT trigger deletion or another destructive action.
  Automatic AdsGuard deletion of user content MUST remain disabled until an
  independently labelled, project-owner-approved corpus evaluation records zero
  false deletions and the project owner separately approves activation for the
  tested policy and scope. A false deletion is a proposed delete decision for a
  logical item labelled legitimate/non-advertising by the approved corpus, not
  merely a count of live deletions. The corpus MUST include legitimate non-exempt
  negatives, exemptions, and eligible advertisement positives; positive functional
  scenarios MUST also pass, so disabling all detection cannot satisfy the gate.
  The corpus, labels, tested decision configuration, evaluation result, and
  activation decision MUST be versioned and auditable. Missing, failed, or
  out-of-scope evidence MUST block automatic deletion, including pending and
  recovered work; decision-affecting changes outside the approved tested scope
  require revalidation and separate activation approval. Provider confidence,
  group-admin mode selection, specification approval, or publishing this document
  alone MUST NOT satisfy this gate. Zero false deletions observed in the approved
  test corpus MUST NOT be represented as a zero-error guarantee in real groups.
- **FR-028**: Warning, deletion, mute-related, informational, rules, and wallet
  interactions MUST be authorized, localized, bounded, and applied at most once per
  logical content item. Token exhaustion MUST NOT introduce a destructive action
  or escalate the configured moderation mode for either a single message or album.
  Warn at most one correlated notice/logicalitem; off/none none. Original-creation caption/forwardgrace300s/120s (0–3600), reply-as-captionOFF, minimum 10 words (0–100), explicit alternate character metric. Deadline correction wins; correction before authorization cancels stale structural grounds not validAD. Confirmed manual mute 24h (1–168) NO associated user deletion. Usercleanup/escalation retired; bot-owned cleanupOFF or30–3600s.

- **FR-029**: Delayed moderation work MUST survive process interruption, have
  exclusive ownership while executing, recover abandoned claims, and expose
  terminal failure for operator action. Recovery MUST check trustworthy available
  evidence before repeating a warning or deletion: confirmed completion MUST NOT
  be repeated; confirmed non-execution permits bounded retry only with current
  authorization and policy safeguards; a still-unknown result MUST be recorded for
  operator review and MUST NOT be retried automatically. Every pending or
  recovered deletion MUST also re-check the current effective deletion mode,
  enablement, eligibility, authorization, exemptions, and FR-027's quality and
  activation gate, including automatic forward/caption guards;
  recovering a claim is not permission to bypass it. FR-064 applies.
  Runnable admission-to-first-dispatch cap 120s, no progress reset. Scheduled grace wait becomes runnable at original due time; recheck current edits/policy/roles/exemptions. Unknown lifecycle never expiry-to-retry.

- **FR-030**: A media album MUST be aggregated and deduplicated as one logical
  moderation item while preserving the identifiers required to apply or recover
  the final action. Known and unknown outcomes of its affected message identifiers
  MUST remain distinguishable; recovery MUST NOT replay completed or unresolved
  parts of an album in violation of FR-029.
  Album<=10, quiet 2s, absolute 5s collection; incomplete/uncertain/oversize blocks destruction. Exact member dispositions/IDs cannot hide legitimate/unknown members or append late members/replies to old delete sets.

- **FR-031**: Administrators MUST be able to list, add, and remove group-scoped
  user and domain allowlist entries with validation and audit.
- **FR-032**: Administrators MUST be able to add positive and negative examples,
  list or summarize them, and clear them only within the selected tenant.
- **FR-033**: Probe, statistics, and threshold simulation MUST clearly identify
  their scope and MUST NOT change policy, moderate content, or spend tokens.
- **FR-034**: Moderation diagnostics MUST minimize retained content and MUST NOT
  reveal private message text, provider credentials, or another tenant's examples
  or decisions.
  Bounded diagnostic/example data follows D-013 provenance/expiry, never a private-text side channel.

#### Tokens and Ledger

- **FR-035**: Token functionality MUST have an auditable global enablement state
  and an explicit effective state for each eligible group journey.
  Global and tenant token switches default OFF; both ON for grants/debits. Disabled balance lookup grants nothing/resets no period.

- **FR-036**: A wallet MUST be uniquely scoped to one tenant and one user, and
  balance lookup MUST NOT create or expose another scope's balance.
- **FR-037**: An eligible user MUST receive at most one weekly grant per tenant and
  grant period, whether triggered lazily, on schedule, or concurrently.
  Grant +1 once CURRENT Monday00:00UTC week to attributable non-bot verified member. Scheduled eligibility checks and explicit balance queries only; AI/Ads/spend never grants implicitly. First eligible grant or SKIPPED_CAP closes week; no later spend/rejoin/restart/toggle reopening or old-period catch-up.

- **FR-038**: A grant MUST NOT raise a wallet above the group's approved carry
  limit.
  Carrydefault 1 integer0–100; cap reduction/import above-cap valid balance never burns/clamps existing balance.

- **FR-039**: A token-consuming logical message or album MUST reduce the wallet at
  most once even when requests, callbacks, or updates repeat concurrently.
  AdsGuard processing in effective no-action mode MUST NOT consume tokens or
  debit its user's wallet, even for a trusted eligible advertisement with
  sufficient balance, any token switches, or a single item/album. Duplicate,
  edited, queued and recovered processing MUST preserve that no-charge rule.
  Before a not-yet-committed debit, the current effective mode MUST be re-checked;
  a profile or stale prior-mode admission cannot authorize charging in no-action
  mode. Replaying a recorded no-action outcome MUST NOT retro-charge it. The
  selected D-008 working contract defines truly distinct identities and charges. Mode changes MUST NOT automatically refund
  historical/already-committed charges; independent authorized grants/corrections
  follow their own approved contracts.
  Only confirmed eligible AD warning/known update costs 1, current warn/both switches ON/atomic balance>=1, at mostonce. none/off/delete/exempt/NON_AD/UNKNOWN/guard-only/mute/diagnostic/AI costs 0. Exhausted warn still warns, no escalation. UNKNOWN forbids NEW debit/repeat, never refunds/resets proven committed debit; preserve delivery/debit separately.

- **FR-040**: Wallet balances MUST never become negative, and every grant, spend,
  correction, or approved reversal MUST have an immutable, reconcilable ledger
  event. An insufficient-balance spending attempt MUST NOT debit the wallet or
  create a successful-spend event; it MUST NOT authorize a moderation-mode override.
  A no-action AdsGuard evaluation MUST NOT create a successful-spend ledger event
  or a debit; zero-spend outcome metadata MUST NOT misrepresent a paid action.
  Trusted origin epoch+tenant+original author+message/albumID stable across edits/caption replies/settings. New admission original NEW Telegrammessage age<=24h/current preservedfence, not forwarded-source date. Recorded insufficient/none/disabled/unknown never retrocharges aftergrant. Compactproof 365d purge only tested input/currentperiod barriers; restart/restore never relabels old inputs.

- **FR-041**: Users MUST be able to view the current tenant balance and, when a
  future grant can change it, a localized indication of the next grant window.
  Explicit confirmed current superadmin correction requires proven erroneous original event, exact linked immutable compensation, redacted reason/audit/nonnegative balance. No automatic old/unknown refund, arbitrary topup/sales feature.

#### Configuration and Localization

- **FR-042**: Effective configuration MUST follow one documented precedence:
  tenant or chat override, global managed value, deployment-provided value, then a
  safe built-in default.
- **FR-043**: Configuration values MUST be typed, bounded, validated before use,
  and rejected without altering the prior value when invalid. Failed persistence
  MUST NOT be acknowledged as a saved change; only confirmed committed values
  may receive success confirmation, without inventing a retry result.
- **FR-044**: Configuration changes MUST be tenant-isolated, auditable, reversible,
  and consistently visible to subsequent work without requiring a source change.
- **FR-045**: Launch readiness MUST include complete user-visible catalogs for
  Persian, English, Arabic, Turkish, and Russian, including buttons, errors,
  administrative text, and plural forms.
- **FR-046**: Missing, stale, or format-incompatible translations MUST block
  release; users MUST NOT see raw translation keys or developer-only fallback text.
- **FR-047**: Messages MUST support Unicode, correct right-to-left or left-to-right
  presentation, locale-aware numbers and time, safe interpolation, and an explicit
  locale fallback chain.
  Locale explicituser/group→managed global→deployment→supported initiating user hint→Persian. Complete five validated catalogs; unsupported choice rejects, corruption safe validated generic localized message+alert never raw key. Administrator-private locale names target group.

#### Data Migration and Cutover

- **FR-048**: Every retained entity MUST have one approved target data contract;
  conflicting legacy table, column, key, or row interpretations MUST be resolved
  explicitly before import.
- **FR-049**: Migration scope MUST account for candidate users, tenant and role
  relationships, settings, non-content session metadata, feedback and unknown
  items, AdsGuard policies and operational work, wallets, grants, ledger events,
  deduplication state, and audit records. D-011 selects preservation of validated
  legacy wallet balances and associated ledger records at first cutover, plus
  only necessary validated linked grant/deduplication evidence that prevents
  duplicate grants or debits. Promotion requires separately approved sanitized
  actual-schema mappings and exact tenant/user reconciliation; this intent alone
  is not permission to execute an import. D-011 now selects other working
  per-entity dispositions, conditional on actual mapping/authority/reconciliation
  approval; D-013 selects separate lifecycle requirements.
  Associated token records MUST NOT promote raw text-bearing notes, credentials
  or context-bearing references merely because they are attached to a ledger.
  The owner-approved first-cutover baseline MUST start with empty conversation
  history and fresh conversational context. Legacy conversation messages, history,
  and session/provider references that would restore historical context MUST be
  intentionally excluded with a reconciliation reason. This exclusion MUST NOT
  delete or modify legacy source data or imply approval of actual import maps.
  D-011 conditional minimal validated IDs/ownership/membership/locale, typed settings, proved DMstates/tenant allowlists; rebuild current roles and reviewed superadmin bootstrap. Start panels/prompts/sessions/history consent/examples empty. Historicalredacted audit grants no current authority; pending metadata NO_EXECUTE/NO_REPLAY not runnable. Actual sanitized maps/reconciliation approval separate.

- **FR-050**: Each imported record MUST pass type, encoding, ownership, tenant,
  timestamp, referential-integrity, and business-invariant validation before
  promotion. Missing/conflicting token identities, schema, ownership, timestamps
  or event outcomes MUST be rejected or quarantined until approved evidence and
  mappings resolve them. Record existence alone MUST NOT prove grant/spend success;
  fabricated identities, opening balances or balancing events MUST NOT conceal
  discrepancies. A validated opening baseline may be used only when its actual
  evidence and reconciliation method are separately approved.
  Quarantineexpiry never erases unexplaineddifferences. Missing current authority/input-fence/checkpoint blocks affected readiness; fresh timestamps/epochs cannot replace proof.

- **FR-051**: Credentials, tokens, passwords, private keys, credential-like values,
  host-specific deployment values, generated artifacts, and corrupt records MUST
  NOT be promoted as business data.
- **FR-052**: Every rehearsal and cutover MUST produce source counts, accepted
  counts, transformed counts, rejected or quarantined counts, integrity totals,
  checksums where applicable, and documented discrepancies by entity and tenant.
  Selected token continuity MUST reconcile preserved balances and associated
  mapped ledger events per tenant/user to their approved actual evidence, with
  independent source/target counts and no unexplained difference. Repeated import,
  lazy/scheduled grants and replay MUST NOT produce a second grant/debit or convert
  a recorded failed spend into success.
- **FR-053**: Production cutover MUST require an approved compatibility window,
  restore-tested backup, rehearsal duration, acceptance thresholds, abort criteria,
  rollback or forward-recovery procedure, and post-cutover sign-off. An unresolved
  token mapping or reconciliation discrepancy MUST block promotion/cutover; the
  legacy source MUST remain unchanged and recoverable.
- **FR-054**: Migration or release interruption MUST NOT allow two owners to apply
  incompatible data changes or allow traffic to use a partially accepted contract.

#### Security, Privacy, and Operations

- **FR-055**: Every read, write, export, administrative action, and background job
  MUST carry an explicit tenant or global scope and enforce least privilege.
- **FR-056**: Commands, callbacks, deep links, imported data, provider input and
  output, URLs, and configuration MUST be treated as untrusted and constrained by
  approved size, format, rate, and destination policies.
  D-012 selects finite input/rate/prompt/queue/album/chunk/resource-test bounds in recommended-contracts.md; unsafe input rejects before provider/effect. Product/reference definitions, not topology/live upstream capacity/provideragreement.

- **FR-057**: Any credential-like value ever committed to a source repository MUST
  be treated as compromised, excluded from migration, and independently revoked or
  rotated before its integration is enabled.
- **FR-058**: Personal and conversation data MUST have approved collection,
  retention, export, correction, and deletion rules and MUST be minimized in logs,
  diagnostics, fixtures, and provider exchanges. New conversation-text storage
  MUST be off by default and require the owning user's explicit opt-in after a
  localized notice explains storage, retention, context use, export, and clearing,
  including the distinction between turning off future storage and clearing
  already retained history. Group, global, or administrator settings MUST NOT
  substitute for that choice.
  Retention MUST be scoped to the owner and tenant; later opt-in MUST NOT backfill
  earlier unconsented text. Every stored question or answer MUST be automatically
  deleted at its original creation time plus 30 days (30 × 24 hours in UTC), or
  earlier when its owner clears it. Reads, edits, exports, and retries MUST NOT
  extend that lifetime. Expired or cleared text MUST NOT remain available through
  history, AI context, exports, feedback, diagnostics, or restored copies.
  Recovery MUST enforce consent, expiry, and clearing before exposing restored
  history. Separate ledger and audit retention MUST NOT become a store for raw
  conversation text. Separate D-013 lifecycles and metadata-only backup terms
  are now selected without changing the accepted history rule. Actual processor
  agreement/region/subprocessor evidence remains required before integration;
  no deletion of original Telegram messages is authorized.
  D-013 selects ordinary metadata30d, logs14d, anonymous aggregates90d, audit/ledger/safety 365d, verifiedoperationalclosure 7d, future staging 7d, quarantine payload 30d. Imported ledger original event clock plus>=365d cutoverterm; exact independently reconciled NON_EVENT checkpoint before expiry. Curated synthetic yearlyreview; permitted own personal example 30d. Encrypted metadata-onlybackups<=30d EXCLUDE history/context/rawexamples/providerpayload: losthistory cannotrestore.

- **FR-059**: Audit evidence MUST be tamper-evident and privacy-preserving, and its
  retention MUST be distinct from optional conversation-history retention.
  Mandatory audit distinct optionalexporters, scoped redacted365d/append-onlycorrection. Outages/unknown effects D-009/013, no fabricatedevidence.

- **FR-060**: Specification, build, test, migration, and runtime tooling MUST remain
  scoped to this project and MUST NOT require host-global installation or alter
  unrelated server services. Current changes MUST be made only in the GitHub
  repository; no server modification, synchronization, container operation,
  migration, or deployment is authorized without new explicit owner permission.
- **FR-061**: Operators MUST be able to distinguish startup, alive, ready, and
  dependency-degraded states without receiving secrets or raw private content.
- **FR-062**: Warmup, weekly grant, cleanup, album, pending moderation, and other
  recurring work MUST be idempotent, bounded, observable, and safe across multiple
  workers. Recurring execution, abandoned-claim recovery, or duplicate updates
  MUST NOT bypass an unresolved external-result hold under FR-064.
- **FR-063**: User requests, provider calls, privileged actions, migrations, and
  jobs MUST be correlated through redacted logs, metrics, traces, and actionable
  alerts.
  Actionable alert PRODUCTION<=300s distinctdelivery/resolution; failed deliveryNOT_DELIVERED, all-sink loss cannotpasscoverage. Required boundedredactedcorrelation remainsmandatory.

- **FR-064**: Dependency outage, quota exhaustion, overload, shutdown, and restart
  MUST degrade safely, preserve accepted durable work, and avoid duplicate
  user-visible outcomes. If the execution result of a Telegram reply, warning, or
  deletion is unknown, recovery MUST first check trustworthy evidence actually
  available. A still-unknown result MUST remain durably recorded for authorized
  operator review without automatic retry, inferred success, or inferred
  non-execution. The record MUST retain logical identity, tenant scope, minimal
  non-content correlation evidence, and the review reason. A timeout or internal
  deduplication record alone MUST NOT prove the external result. Read-only
  reconciliation checks remain permitted; the no-retry rule applies to repeating
  the external action while its execution result remains unknown. Referral does
  not itself authorize manual replay, a destructive action, or a token refund; any
  separately authorized follow-up MUST remain audited and preserve current
  authorization, policy, and ledger deduplication safeguards.
  Current scopedSA/operator ownsreview; groupadmin onlyownmoderationmetadata. After30d archive UNRESOLVED_NO_REPLAY; compact365d expiry behind proven age/origin/currentperiod barriers. Critical acknowledged wallet/audit/replay/security/privacy/policyRPO 0 withinapprovedfailureenvelope; ordinary metadata RPO <=24h excludescriticalstates. Missing proof fails closed; lost consent defaultsOFF. Restore requires exact critical checkpoint/fence reconciliation, metadata-only backup cannot restore lost history.

- **FR-065**: Each release MUST be repeatable from reviewed project inputs and MUST
  include owned deployment, backup, restore, incident-response, reconciliation,
  and rollback procedures that do not depend on mutable host state.

### Scope Boundaries

**In scope**:

- The complete approved Telegram bot behavior represented by BR-001 through BR-019.
- Security, privacy, operational, data-migration, and deployment outcomes required
  to release those behaviors globally.
- Traceable retirement of BR-020 through BR-023.
- A compatibility baseline that later child specifications may refine without
  weakening this contract.

**Out of scope for this specification**:

- Modifying `ai_bot6` or its live deployments.
- Choosing detailed architecture, package layout, frameworks, storage products,
  provider protocols, or deployment topology; those belong to the approved plan.
- Adding non-Telegram user channels or unrelated new business capabilities.
- Executing production migration or cutover.
- Writing production code before specification and plan approval.

### Key Entities

- **User**: A Telegram participant with identity, locale hints, direct-message
  allowance state and a distinct explicit private-conversation denial. Denial
  overrides ordinary allowances until current superadmin explicit unblock;
  absence of an allowance is not evidence of a historical block. Tenant-specific
  participation and an explicit owner-scoped history-storage choice with policy-
  notice acknowledgment remain independent; absence of explicit history opt-in
  means text storage is off.
- **Tenant / Group**: The isolation boundary for members, administrators, policy,
  configuration, conversation behavior, moderation, tokens, and reporting.
- **Role Assignment**: A current, scoped authorization relationship for regular
  user, group administrator, anonymous administrator, or superadmin behavior.
- **Administrator Group Context**: A private-panel binding between one
  administrator and one currently selected manageable group, including expiry and
  authorization state.
- **Conversation Session**: An owner- and tenant-scoped continuity boundary with
  lifecycle, provider correlation, and eligible new-deployment history. Durable
  conversation text and context-bearing references require the owner's opt-in;
  expired or cleared text cannot be restored by a session reference. The first
  cutover uses fresh conversational context; no legacy context-bearing reference
  may restore an old conversation.
- **Conversation Record**: An eligible user question or assistant answer stored
  only after its owner explicitly opts in after notice, with owner, tenant,
  original creation time, and expiry 30 days later. The owner can clear it earlier;
  access, edits, exports, and retries do not reset expiry.
- **Feedback / Unknown Item**: A response evaluation or unresolved question linked
  to an eligible session, tenant, actor, report state, and audit history.
- **Configuration Value**: A typed value with scope, precedence, revision, actor,
  validation result, and effective period.
- **Ads Policy**: Per-tenant moderation enablement, action, threshold, exemption,
  cooldown, cleanup, and provider-selection rules. Automatic deletion additionally
  depends on auditable, scope-matching corpus evaluation and separate project-owner
  activation approval; an enabled group or delete-mode setting is not that approval.
- **Ads Decision**: The explainable classification and policy outcome for one
  logical content item, including exemption, confidence, status, and correlation.
- **Ads Example**: A tenant-scoped labeled example used to improve classification,
  with source, label, actor, and lifecycle.
- **Ads Allowlist Entry**: A tenant-scoped approved user or domain exemption with
  actor and lifecycle.
- **Media Album**: A tenant-scoped logical content item composed of one or more
  message identifiers and a bounded aggregation lifecycle.
- **Pending Moderation Action**: Durable, exclusively owned work with target,
  schedule, attempts, status, recovery state, and terminal outcome. Recovery
  distinguishes confirmed completion, confirmed non-execution, and an unresolved
  external result held for operator review; an unresolved result is not success
  or permission to repeat the action.
- **Wallet**: A non-negative balance uniquely owned by one tenant and one user.
  First-cutover continuity preserves validated legacy balances only after actual
  identity/schema mapping and exact reconciliation are separately approved.
- **Weekly Grant**: An idempotent grant eligibility and outcome for one tenant,
  user, and grant period.
- **Ledger Event**: An immutable grant, spend, correction, or reversal with amount,
  cause, deduplication reference, actor when applicable, and time.
- **Deduplication Record**: A scoped identity proving that a logical request, job,
  moderation action, grant, or spend has already been accepted. Acceptance alone
  does not prove Telegram execution or delivery.
- **Audit Event**: A redacted, tamper-evident record of a privileged attempt or
  security-relevant outcome.
- **Localization Catalog**: A versioned set of complete user-visible messages,
  variables, plural forms, formatting rules, and directionality for one locale.
- **Migration Batch**: A pinned source snapshot plus mappings, validation results,
  counts, checksums, rejected records, approvals, and cutover status.
- **Reconciliation Report**: The entity- and tenant-level proof that accepted
  source data and resulting business invariants match the approved migration
  contract.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Before planning approval, 100% of discovered observable legacy
  behaviors are linked to evidence, one preserve/change/retire decision, an owner,
  and at least one verification scenario; zero items remain unclassified.
- **SC-002**: Before cutover, 100% of approved P1 acceptance scenarios pass against
  representative legacy-derived fixtures, and every intentional difference has
  explicit project-owner approval.
- **SC-003**: Under D-012's selected Standard synthetic workload (50 tenants,
  20 users each; 5 steady and 25 burst logical journeys/second), at least 99% of
  eligible interactive admissions show confirmed progress/acknowledgment within
  2 seconds, and at least 95% reach a confirmed answer or approved safe terminal
  outcome within 20 seconds. Interactive journeys are the declared conversation,
  wallet, administration and explicit history/feedback mix (70%); passive AdsGuard
  has separate decision/action clocks and MUST NOT receive acknowledgment spam.
  Measure each of three frozen runs independently using measurement-contract.md:
  include queue/retry time and unknown/lost work in denominators, never infer
  Telegram completion from enqueue/dedupe, never turn extra drain time into a
  timely pass, and never pass healthy valid questions with fast errors alone.
  Upstream outages are predeclared separate fault runs, not post-hoc exclusions.
  Unresolved operator-review items remain reported separately and MUST NOT count
  as confirmed answers or safe terminal outcomes. This selected controlled
  reference is NOT measured real Telegram/provider capacity; final input/rate/
  resource manifests and actual evaluation remain separate gates.
- **SC-004**: In authorization and adversarial testing, 100% of unauthorized
  privileged attempts are denied, and zero tests disclose or modify another
  tenant's configuration, conversation, moderation, token, or audit data. Privacy
  tests create zero durable history-text records without explicit owner opt-in,
  verify automatic deletion at each message's 30-day boundary or earlier clearing,
  and expose zero expired or cleared records through history, exports, AI context,
  or restore. Those checks preserve separately governed ledger and audit records.
  Explicit private-conversation denial tests MUST deny every ordinary
  global/environment/managed allowance combination, every private input route
  and not-yet-executed delayed dispatch; only current superadmin explicit unblock
  clears denial. Plain allow/global changes, restart and unauthorized unblock
  produce zero bypasses. Other access/feature gates still apply after unblock.
- **SC-005**: Across the independently labelled, project-owner-approved moderation
  corpus, proposed delete decisions yield zero false deletions of legitimate
  items, every exempt or insufficient-confidence item avoids destructive action,
  and eligible advertisement cases pass their expected functional scenarios.
  Every expected action occurs at most once; repeated delivery creates zero
  duplicate warnings, deletions, or charges. Gate tests produce zero automatic
  deletion attempts for new, queued, or recovered content when quality evidence or
  separate project-owner activation approval is missing, failed, or out of scope.
  Exhausted-wallet tests for single messages and albums produce zero deletions in
  warning or no-action mode, no moderation action in no-action mode, and no
  successful token debit on insufficient-balance attempts. No-action processing
  MUST also produce zero AdsGuard debit/successful-spend events with sufficient
  balance, token switches, singles/albums and duplicate/edit/recovered variants.
  The delegated working contract additionally selects confirmed-warning-only
  charging and zero delete charges. Zero observed corpus
  errors is an activation criterion, not a guarantee of zero real-world errors.
  Automatic forbidden-forward and captionless-media guard tests MUST produce zero
  user-content deletions in disabled/off, no-action or warning states, including
  profile combinations and queued/recovered actions whose effective mode changes
  before execution. Deletion-mode cases remain conditional on all normal safeguards
  and the separate quality/activation gate; D-007 supplies the exact working
  grace/eligibility matrix, not permission to activate or guessed gold labels.
- **SC-006**: Across concurrent grant and spending tests, wallet balances never
  become negative, each eligible weekly grant occurs at most once, each logical
  spend occurs at most once, and ledger totals reconcile exactly to wallet changes.
  No-action AdsGuard tests MUST produce zero attributable spend/debit and zero
  successful-spend events for sufficient/zero wallets, any token switches,
  singles/albums, duplicate/edit processing and queued/recovered mode changes.
  Independent authorized grants are separately reconciled; an overall wallet
  change is not confused with a charge caused by the no-action journey.
- **SC-007**: Every migration rehearsal accounts for 100% of source rows in scope
  as imported, transformed, rejected, quarantined, or excluded; zero secret values
  or legacy conversation histories are promoted, no historical conversational context is
  restored, and all separately approved financial-like totals reconcile without
  unexplained difference. D-011 token continuity preserves validated source
  wallet balances and associated ledger records against separately approved
  actual-schema evidence per tenant/user; required linked grant/deduplication
  evidence prevents duplicate grant/debit and preserves actual failed outcomes.
  Unknown mappings, fabricated balancing events or unexplained differences cannot
  pass. D-011 selects other conditional per-entity dispositions, not actual
  source maps, evidence or import execution approval.
- **SC-008**: A full backup restoration and cutover rollback drill completes within
  the D-012 selected 60-minute combined test window, from operator go through
  verified readiness including reconciliation and traffic gating, and restores all
  release-critical user,
  tenant, moderation, token, and audit invariants while enforcing conversation
  consent, expiry, clearing, current denials/revocations and deletion-safety gates
  before traffic. Metadata-only backups intentionally contain no history/context;
  lost history cannot be recovered from them. Critical acknowledged invariants
  target RPO0 within the plan-approved failure envelope, ordinary metadata <=24h;
  missing critical evidence fails closed and lost consent defaults OFF.
- **SC-009**: 100% of user-visible keys used by approved launch journeys are
  complete and format-valid in Persian, English, Arabic, Turkish, and Russian, with
  zero raw keys or developer-only messages visible in acceptance testing.
- **SC-010**: At least 45 of 50 representative native/fluent participants complete
  one assigned primary first-use journey without operator assistance, with at
  least 9 of 10 in each launch locale. Each locale includes five regular users and
  five group administrators; report both role cohorts and every attempted,
  abandoned or assisted failure using the selected measurement contract. No
  stronger per-role passing threshold is inferred; no study has been run.
- **SC-011**: During interruption and duplicate-delivery tests, every accepted
  durable item reaches a verified completion, safe authorized resumption, or
  durably recorded operator-review disposition within 5 minutes of service
  restoration, with zero duplicate user-visible or ledger effects. For unresolved
  Telegram replies, warnings, or deletions, tests verify zero automatic repeat
  attempts while the external result remains unknown, including through restarts
  and duplicate updates. If sufficient evidence is unavailable within the recovery
  target, a durable review disposition is valid; referral is not proof of external
  completion and does not set a five-minute deadline for human resolution.
- **SC-012**: Every critical dependency/security/privacy/queue-age/migration/
  reconciliation injection produces an owned redacted actionable alert within
  300 seconds of its declared detectable threshold. Production, delivery and
  human resolution are distinct; failed delivery is NOT_DELIVERED. All-sink loss
  cannot earn audit/alert coverage. Queue age is 120s for runnable work; required
  observability/admission evidence must operate or affected work stops.
- **SC-013**: A release and rollback can each be completed from reviewed
  project-scoped inputs within 30 minutes, without installing host-global tools or
  changing unrelated server services.
- **SC-014**: With no measured legacy 30-day peak available, the selected Standard
  substitute MUST also pass at 2× arrival rates, tenant/user population and
  synthetic dataset while preserving users per tenant, mix, payload bounds,
  duplicate percentage and clock/dependency contract. Its 100 tenants/2,000 users,
  10 steady/50 burst journeys per second produce 21,600 unique offered measured
  journeys plus 4,320 duplicate deliveries, versus 10,800 plus 2,160 at base.
  Every frozen base and 2× run MUST meet SC-003 with zero lost accepted work or
  unexpected rejection of valid reference input. This is not 2× an observed legacy
  peak or evidence of actual upstream capacity; final manifests and passing
  evaluations remain future gates.

## Assumptions

- The read-only evidence baseline is the `ai_bot6` default branch at commit
  `cdb0efd4b39eaafd1265822350606887fe3b40b3` as inspected on 2026-08-17. Later
  legacy changes are out of baseline until explicitly reviewed and added.
- The rewrite remains a Telegram bot serving private chats, groups, supergroups,
  group topics, callbacks, and administrator private-panel journeys.
- Preserve is a capability-level default proposal only; surfaced source-branch
  differences remain unresolved until recorded and approved. The behavior ledger
  and decision register expose security, route, destructive-guard and spending
  differences rather than silently accepting unsafe legacy choices. Internal
  structure and unsafe deployment behavior are not compatibility contracts.
- The first cutover targets the currently represented legacy deployment and its
  approved tenants, while the resulting product must support independent future
  deployments without source forks or host-global installation.
- Persian, English, Arabic, Turkish, and Russian are launch locales because the
  legacy interface exposes all five selectors. A locale is not considered supported
  until its complete catalog passes acceptance.
- The owner has decided that the first cutover starts with empty conversation
  history and fresh conversational context. Legacy history is intentionally
  excluded without deleting or modifying the old data; other entity-import
  decisions remain separate. For new conversation text, the owner approved
  default-off storage, explicit user opt-in after notice, automatic deletion of
  each message 30 days after its original creation, and owner-scoped export or
  earlier clearing. The decision does not approve a provider retention contract,
  a backup retention duration, or other personal-data lifecycles; those remain
  separate approval obligations and must not bypass the approved history policy.
- Legacy data access will use sanitized snapshots or controlled exports. Live
  production credentials and raw secrets are neither required nor permitted for
  specification and test work.
- External Telegram and AI-answer/classification services remain dependencies, but
  their product-facing failures must follow this specification and no provider's
  internal model becomes a user contract.
- When an undocumented legacy behavior is discovered, it is added to the
  classification ledger rather than inferred as approved.
- Detailed capacity limits, retention durations for data outside the approved
  30-day conversation-text policy, and cutover windows will use measured legacy
  evidence where available and explicit owner-approved baselines otherwise.
- The owner has approved respecting AdsGuard's configured mode when tokens are
  exhausted: warning only warns, no-action performs no moderation action, and
  deletion remains limited to deletion mode with the normal safeguards. This
  intentionally changes the legacy exhaustion override. The later D-008 answer
  separately approves no token consumption by no-action AdsGuard; warning/deletion
  charges and other economic/replay/correction rules remain unapproved. The owner also approved zero false deletions in an
  independently labelled, owner-approved test corpus and separate explicit
  owner activation approval as necessary conditions for automatic deletion.
  Neither a corpus evaluation result nor an activation approval is provided by
  this clarification. The corpus and tested decision scope still need definition
  and approval; provider confidence alone is insufficient, and zero observed
  test errors does not imply zero real-world errors.
- The owner approved D-006's explicit private-conversation deny rule: /block
  overrides ordinary global/environment/managed allowances until current
  superadmin explicit unblock. Plain allow/global toggles and cached admissions
  cannot bypass it; unblock does not grant an allowance or remove other gates.
  This intentionally changes legacy revocation-only behavior and requires clear
  administrator/user communication before later authorized activation. Historical
  absence/deletion of an allow row is not proof of a block; source data remains
  untouched and any initial access-state import still requires D-011 approval.
  D-010 now selects the new private unblock panel and exact working interaction
  contracts; group roles, moderation and history rights are not revoked by
  this private-conversation decision. No application code or deployment is approved.
- The owner approved D-007's automatic forward/caption mode rule: these guards
  respect the group's current effective mode and cannot independently delete in
  disabled/off, no-action or warning states. Delete-mode eligibility, exemptions,
  authorization and the separate quality/activation gate still apply, including
  at queued/recovered execution. This intentional change removes legacy
  pre-mode deletion bypasses; group administrators and affected users must be
  informed before later authorized activation. No legacy data changes, deletion
  activation or manual-mute/user-chat-cleanup approval follows from this answer;
  remaining D-007 content/grace/notice/mute/cleanup working choices are selected
  under recommendation delegation, not this historical individual answer.
- The owner approved D-008's no-action/no-charge rule: AdsGuard in no-action
  mode spends no tokens even for trusted eligible ads with sufficient balance.
  Single/album, duplicate/edit, queued/recovered and current-mode-at-debit cases
  preserve that scoped zero-charge outcome. No-action is not the same as turning
  AdsGuard off; this answer does not authorize a provider-processing/privacy path
  or decide classification itself. Independent approved grants are not prohibited;
  historical/already-committed debits are not automatically refunded. Remaining
  economic/replay/correction working choices are now selected by delegated
  D-008: only confirmed eligible warning costs1, grants+1/currentweek/carry1,
  no implicit grant/backfill or retro-charge. Publication does not implement it.
- The owner selected D-011 token continuity: preserve validated legacy wallet
  balances and associated ledger records, plus only necessary validated linked
  grant/deduplication evidence, conditional on separately approved actual sanitized
  mappings and exact tenant/user reconciliation. Missing/conflicting evidence is
  rejected/quarantined, not fabricated; historical debits are not rewritten or
  automatically refunded. No other entity import, retention policy, source deletion
  or actual migration is approved by this selection. Old history/context remains
  excluded and unresolved Telegram effects remain held under FR-064.
- The owner has approved checking available trustworthy evidence for a Telegram
  reply, warning, or deletion whose result is unknown, then holding a still-unknown
  result for operator review without automatic retry. Unknown is neither completed
  nor confirmed not executed; referral does not authorize manual repetition. No
  Telegram result-lookup or external exactly-once guarantee is assumed. SC-011
  measures a recovered disposition or review referral, not guaranteed external
  completion or a human-resolution deadline. Internal grants, spends, and ledger
  reconciliation retain their existing deduplication invariants; uncertainty does
  not imply an automatic refund or reversal.
- Under the owner's recommendation delegation, D-009's documented security
  normalization is selected: current scoped authorization for reads/writes,
  callbacks/prompts/jobs; original target-bound replies; superadmin-only global
  writes; scoped unknown review; no false save confirmation; redacted health.
  D-009/010/012/013 now define exact working interactions, bounds, outage,
  review ownership and evidence lifecycles, pending whole-spec approval. Security
  invariants are not weakened and no silent failure/bypass is approved.
- Under the same delegation, select D-012's Standard synthetic substitute and
  existing numerical counting/clocks, three independent base/2× repetitions,
  100×20 race trials, 50-person/locale first-use study and 60-minute combined
  restore/rollback test window; separate release/rollback tests remain 30 minutes
  each. These are definition choices, NOT RUN/PASS or real capacity claims. The
  2× profile doubles rates, tenant/user population and synthetic data, not an
  unknown legacy peak. D-012 now supplies finite payload/rate/resource/queue/
  grace and route definitions and >=1,000 held-out corpus coverage targets;
  actual frozen manifest/gold labels and independently approved evidence remain
  future evaluation gates.
  Corpus owner sign-off, actual passing evaluations and separate deletion
  activation are later gates; delegation does not supply them. Full spec and
  architecture/migration plan remain unapproved, with no code/server permission.

### Dependencies

- Continued read-only access to the pinned `ai_bot6` source revision.
- A sanitized, production-like legacy data snapshot and entity counts before
  migration planning and rehearsal.
- Project-owner decisions for every proposed change or retirement and any newly
  discovered behavior.
- Test bot identities, representative test groups, and accounts for regular user,
  group administrator, anonymous administrator, removed administrator, and
  superadmin scenarios.
- Independently rotated deployment credentials before any affected integration is
  enabled.
- Approved provider-processing, backup-content, other personal-data retention,
  service-level, maintenance-window, and data-residency contracts before
  architecture-plan approval; none may bypass the approved opt-in 30-day history
  policy.
