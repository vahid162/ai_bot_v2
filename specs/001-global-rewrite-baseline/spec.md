# Feature Specification: Global Rewrite Behavioral Baseline

**Feature Branch**: `main`

**Created**: 2026-08-17

**Status**: Draft

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
Evidence-to-scenario coverage and the owner decisions below remain incomplete;
the requirements checklist records those limits.

## Clarifications

### Session 2026-10-11

- Q: Should the first cutover migrate legacy conversation history or start the new
  version with empty history? → A: Start the new version with empty conversation
  history. This does not authorize deleting legacy data or migrating other entity
  types.

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
journeys while verifying tenant and user isolation.

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
   request completes, **Then** the user receives a safe outcome without duplicated
   messages, leaked provider data, or cross-session history.
6. **Given** a user requests history export or clearing, **When** ownership is
   verified, **Then** only that user's eligible history in the selected scope is
   exported or cleared and the result is confirmed. At the first cutover, this
   history starts empty; subsequent eligible exchanges in the new deployment
   remain subject to the approved retention policy.

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

**Independent Test**: Run a labeled corpus of text, captions, replies, forwards,
contact-like content, allowlisted content, and albums through each policy mode,
including provider failure and repeated delivery.

**Acceptance Scenarios**:

1. **Given** AdsGuard is enabled with a configured threshold, **When** eligible
   content is classified above that threshold, **Then** exactly the configured
   action is scheduled or applied and the decision is recorded.
2. **Given** content from an exempt administrator or allowlisted user, or content
   matching an approved domain or reply exemption, **When** it is evaluated,
   **Then** no destructive moderation action occurs and the exemption reason is
   traceable.
3. **Given** a media album arrives as several updates, **When** it is classified
   and acted upon, **Then** it is treated as one logical item with at most one
   warning, one charge, and one final policy outcome.
4. **Given** a delayed action worker stops after claiming work, **When** another
   worker recovers the item, **Then** the action resumes safely without duplicate
   deletion, warning, or charge.
5. **Given** an administrator runs probe, statistics, or threshold simulation,
   **When** results are displayed, **Then** no real moderation action or token
   spend occurs.
6. **Given** classification is unavailable, malformed, or below the approved
   confidence threshold, **When** policy is evaluated, **Then** the system does
   not perform a destructive action and records an operator-visible reason.

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
5. **Given** a successful cutover, **When** post-cutover checks run, **Then** the
   separately approved balances, grants, moderation work, roles, tenant counts, and
   non-content session metadata reconcile to their approved baselines, while new
   conversation history and conversational context start empty.
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
   requested, **Then** no negative balance is possible and the user receives
   localized guidance.
5. **Given** the same user participates in two groups, **When** balances are
   viewed, **Then** each tenant's wallet, grant, and ledger remain independent.

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
timeout, worker interruption, duplicate update delivery, backup restore, release,
and rollback under the approved production-equivalent security boundary.

**Acceptance Scenarios**:

1. **Given** a critical dependency is unavailable, **When** readiness is checked,
   **Then** the service reports not ready without leaking credentials or private
   messages while unrelated host services remain unaffected.
2. **Given** an update or job is retried after interruption, **When** processing
   resumes, **Then** correlation and deduplication make the outcome traceable and
   prevent duplicated user-visible effects.
3. **Given** a release or rollback, **When** the operator follows the approved
   procedure, **Then** only project-scoped resources change and no global host tool
   installation is required.
4. **Given** a security, availability, queue-age, or reconciliation threshold is
   breached, **When** monitoring evaluates it, **Then** an actionable, redacted
   alert points to an owned recovery procedure.

### Edge Cases

- Telegram delivers the same update more than once, out of order, or after a
  callback has expired.
- A group is upgraded, renamed, migrated to a new identifier, deleted, or removes
  the bot while an administrator still has an active private-panel context.
- A user opens multiple panel messages or switches the active group while a prior
  force-reply or callback remains outstanding.
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
  already in flight.
- Weekly grant, lazy grant, token spend, and duplicate cleanup run concurrently
  across multiple workers.
- A delayed moderation worker dies after claiming an item but before completing its
  external action or recording completion.
- A language catalog is unavailable, contains a formatting mismatch, or receives an
  unsupported locale tag.
- A legacy export contains duplicate identities, orphaned rows, impossible
  timestamps, invalid encodings, negative balances, conflicting tenant ownership,
  or schema variants not present in the approved mapping.
- A backup is incomplete, stale, encrypted with an unavailable key, or cannot be
  restored within the approved cutover window.
- Secret scanning detects a credential-like value in legacy data, documentation,
  build input, logs, or release artifacts.
- Audit, metrics, or error-reporting sinks are unavailable or apply backpressure.
- The service reaches a configured rate, concurrency, queue, or provider quota.

## Requirements *(mandatory)*

### Behavioral Scope and Classification

The following classification is the proposed rewrite baseline. `Preserve` means
retain the approved externally observable outcome, not the legacy internal design.
`Change` and `Retire` entries require explicit project-owner approval before they
become binding.

| ID | Legacy capability or element | Proposed classification | Source evidence | Required outcome |
|----|------------------------------|-------------------------|-----------------|------------------|
| BR-001 | Start, help, identity, scoped command menus, and health journeys | Preserve | `bot.py` and user command registrations | Equivalent discoverability and safe responses by user scope |
| BR-002 | Private AI questions, `/ask`, reply input, typing state, and direct answers | Preserve | `user_commands.py` | One isolated, localized outcome per accepted request |
| BR-003 | Group AI enablement, mention/all modes, administrator-only mode, throttling, and autocleanup | Preserve | `user_commands.py` and `panel_ui.py` | Per-group policy with no response outside eligible triggers |
| BR-004 | Conversation sessions, local history, clear, and export | Preserve capabilities; first-cutover history exclusion approved by owner | `user_commands.py` and `shared_utils.py` | Empty initial history and fresh AI context; owner-scoped continuity, export, and deletion for new-deployment history |
| BR-005 | Feedback, unknown-question capture, user reporting, and administrator review | Preserve | callback registrations and user/admin commands | Traceable feedback without cross-tenant disclosure |
| BR-006 | Global direct-message switch, allow/block policy, and user status listing | Preserve | `admin_commands.py` | Deny-by-default access with explicit, auditable exceptions |
| BR-007 | Private management panel, group binding, active-group selection, deep-link management, and force-reply editing | Preserve | `user_commands.py` and `panel_ui.py` | Fresh authorization and isolated administrator context |
| BR-008 | Group-admin and superadmin roles, superadmin membership, and privileged audit | Preserve | `admin_commands.py` and `shared_utils.py` | Least privilege, current-role verification, and complete audit |
| BR-009 | Per-group chat, AdsGuard, language, and command-menu controls | Preserve | `panel_ui.py`, `bot.py`, and admin commands | Validated settings with predictable scope and menus |
| BR-010 | Ads classification of text, captions, replies, forwards, contact-like content, domains, and media albums | Preserve | `ads_guard.py` and AdsGuard command registration | One explainable decision per eligible logical item |
| BR-011 | AdsGuard off/none/warn/delete policy, threshold, cooldown, and cleanup behavior | Preserve | `ads_guard.py`, `ads_commands.py`, and `panel_ui.py` | Exact per-group enforcement with safe non-destructive failure |
| BR-012 | Durable pending moderation, retries, stuck-work recovery, and album cleanup | Preserve | `ads_pending_jobs.py` and `ads_album_jobs.py` | Recoverable, idempotent action with bounded retry |
| BR-013 | Ads examples, positive/negative labeling, user/domain allowlists, probe, statistics, simulation, and private debug control | Preserve | `ads_commands.py` registrations | Tenant-scoped admin tools; diagnostic actions remain side-effect free |
| BR-014 | Tenant wallets, balance aliases, weekly grants, and carry limits | Preserve | `tokens/handlers.py`, `tokens/jobs.py`, and `tokens/models.py` | One grant per tenant/user/week and accurate balance |
| BR-015 | Token spending, logical-message deduplication, and ledger history | Preserve | `tokens/models.py` | Exactly-once charge, non-negative balance, auditable ledger |
| BR-016 | Database-first configuration with deployment fallback | Change | configuration readers and startup seeding | One uniform precedence chain with validation and safe defaults |
| BR-017 | Persian, English, Arabic, Turkish, and Russian selectors and fallback | Change | language callback and message catalogs | Preserve selectors while requiring complete catalogs and no raw-key fallback |
| BR-018 | Structured logging, metrics, health reporting, optional error reporting, provider warmup, and recurring cleanup | Preserve | `bot.py`, logging, monitoring, and job modules | Redacted, actionable operations with idempotent jobs |
| BR-019 | Legacy operational data and schema history | Change | migrations and runtime queries | Import only validated business data into one approved data contract |
| BR-020 | Schema upgrades during application startup and competing migration entry points | Retire | deployment and migration startup configuration | One separately controlled, observable migration owner |
| BR-021 | Committed credentials, mutable release inputs, host-specific paths, fixed identifiers, and shared global deployment assumptions | Retire | tracked deployment configuration | Rotated secrets and isolated, portable project configuration |
| BR-022 | Large mixed-responsibility modules and direct coupling between transport, policy, provider calls, and data access | Retire | legacy module boundaries | Preserve outcomes without preserving unsafe internal structure |
| BR-023 | Generated bytecode and other runtime artifacts tracked as source | Retire | legacy repository tree | Generated or local runtime artifacts never enter reviewed source |

Any newly discovered behavior is `Unclassified` by default and blocks dependent
planning until it is added to this table or a child specification with evidence and
an owner-approved decision.

The table is an inventory, not a completed traceability ledger. Before approval,
each entry needs an exact source location at the pinned revision, acceptance
scenario references, the deciding owner, and the impact/transition details required
by FR-005. Capability classifications remain proposals; the first-cutover history
exclusion recorded in Clarifications is an owner-approved decision, not approval
of the full capability inventory.

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

This is source evidence, not an approved rewrite outcome. The existing wallet
scenarios do not fully specify these moderation consequences. Owner approval and
acceptance coverage are required before dependent planning; selecting different
outcomes requires an explicit `Change` decision.

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
- **FR-009**: Authorized users MUST be able to submit questions through ordinary
  eligible messages, `/ask`, and supported reply journeys and receive one
  correlated outcome.
- **FR-010**: Each group MUST independently support disabled, explicit-address, and
  all-eligible-message conversation behavior, plus an optional
  administrators-only restriction and bounded response frequency.
- **FR-011**: Conversation context MUST be isolated by tenant, user, and applicable
  topic or session so that content cannot cross those boundaries.
- **FR-012**: Users MUST be able to clear and export only the conversation history
  they own and are permitted to access, with an explicit result. For the new
  deployment, this starts empty at the first cutover; legacy history MUST NOT
  appear through these journeys.
- **FR-013**: Accepted requests MUST provide timely progress indication without
  producing unbounded or orphaned activity indicators.
- **FR-014**: Provider timeout, invalid output, rate limit, or outage MUST result in
  a safe localized outcome and MUST NOT duplicate replies or expose provider
  internals.
- **FR-015**: Users MUST be able to record positive or negative feedback and report
  an unknown answer; administrators MUST be able to review only unknown items
  within their authorized scope.

#### Identity, Administration, and Tenancy

- **FR-016**: The product MUST distinguish regular users, group administrators,
  anonymous group administrators, and superadmins with deny-by-default privileges.
- **FR-017**: Authorization MUST be re-evaluated against current scope for every
  privileged command, callback, force-reply, and delayed action.
- **FR-018**: A verified administrator MUST be able to open a private group picker,
  select an active manageable group, and see which group a subsequent action will
  affect.
- **FR-019**: Active-group and panel state MUST be isolated per administrator and
  MUST expire or fail safely when stale, malformed, or no longer authorized.
- **FR-020**: Group administrators MUST be able to view and change the approved
  chat, AdsGuard, token, language, and cleanup settings for their own group.
- **FR-021**: Superadmins MUST be able to view and manage global direct-message
  policy, explicit user access, superadmin membership, global feature switches, and
  approved diagnostic controls only from an authorized context.
- **FR-022**: Command menus and management entry points MUST reflect the actor's
  current scope without granting authority by visibility alone.
- **FR-023**: Every privileged attempt, whether accepted or denied, MUST produce a
  redacted audit event with actor, target scope, action, result, reason, time, and
  correlation identifier.

#### AdsGuard

- **FR-024**: Each group MUST have independent AdsGuard enablement, action mode,
  confidence threshold, request frequency, training-example limit, and cleanup
  policy.
- **FR-025**: AdsGuard MUST evaluate all approved eligible content forms and MUST
  apply administrator, user, domain, reply, contact-like, and other approved
  exemptions before any destructive action.
- **FR-026**: Every decision MUST record the logical content identity, tenant,
  policy revision, classification outcome, confidence when available, exemption or
  action reason, and final status without retaining unnecessary message content.
- **FR-027**: Missing, malformed, untrusted, timed-out, or insufficient-confidence
  classification MUST NOT trigger deletion or another destructive action.
- **FR-028**: Warning, deletion, mute-related, informational, rules, and wallet
  interactions MUST be authorized, localized, bounded, and applied at most once per
  logical content item.
- **FR-029**: Delayed moderation work MUST survive process interruption, have
  exclusive ownership while executing, recover abandoned claims, use bounded
  retry, and expose terminal failure for operator action.
- **FR-030**: A media album MUST be aggregated and deduplicated as one logical
  moderation item while preserving the identifiers required to apply or recover
  the final action.
- **FR-031**: Administrators MUST be able to list, add, and remove group-scoped
  user and domain allowlist entries with validation and audit.
- **FR-032**: Administrators MUST be able to add positive and negative examples,
  list or summarize them, and clear them only within the selected tenant.
- **FR-033**: Probe, statistics, and threshold simulation MUST clearly identify
  their scope and MUST NOT change policy, moderate content, or spend tokens.
- **FR-034**: Moderation diagnostics MUST minimize retained content and MUST NOT
  reveal private message text, provider credentials, or another tenant's examples
  or decisions.

#### Tokens and Ledger

- **FR-035**: Token functionality MUST have an auditable global enablement state
  and an explicit effective state for each eligible group journey.
- **FR-036**: A wallet MUST be uniquely scoped to one tenant and one user, and
  balance lookup MUST NOT create or expose another scope's balance.
- **FR-037**: An eligible user MUST receive at most one weekly grant per tenant and
  grant period, whether triggered lazily, on schedule, or concurrently.
- **FR-038**: A grant MUST NOT raise a wallet above the group's approved carry
  limit.
- **FR-039**: A token-consuming logical message or album MUST reduce the wallet at
  most once even when requests, callbacks, or updates repeat concurrently.
- **FR-040**: Wallet balances MUST never become negative, and every grant, spend,
  correction, or approved reversal MUST have an immutable, reconcilable ledger
  event.
- **FR-041**: Users MUST be able to view the current tenant balance and, when a
  future grant can change it, a localized indication of the next grant window.

#### Configuration and Localization

- **FR-042**: Effective configuration MUST follow one documented precedence:
  tenant or chat override, global managed value, deployment-provided value, then a
  safe built-in default.
- **FR-043**: Configuration values MUST be typed, bounded, validated before use,
  and rejected without altering the prior value when invalid.
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

#### Data Migration and Cutover

- **FR-048**: Every retained entity MUST have one approved target data contract;
  conflicting legacy table, column, key, or row interpretations MUST be resolved
  explicitly before import.
- **FR-049**: Migration scope MUST account for candidate users, tenant and role
  relationships, settings, non-content session metadata, feedback and unknown
  items, AdsGuard policies and operational work, wallets, grants, ledger events,
  deduplication state, and audit records; their imports require separate approval.
  The owner-approved first-cutover baseline MUST start with empty conversation
  history and fresh conversational context. Legacy conversation messages, history,
  and session/provider references that would restore historical context MUST be
  intentionally excluded with a reconciliation reason. This exclusion MUST NOT
  delete or modify legacy source data or imply approval of other entity imports.
- **FR-050**: Each imported record MUST pass type, encoding, ownership, tenant,
  timestamp, referential-integrity, and business-invariant validation before
  promotion.
- **FR-051**: Credentials, tokens, passwords, private keys, credential-like values,
  host-specific deployment values, generated artifacts, and corrupt records MUST
  NOT be promoted as business data.
- **FR-052**: Every rehearsal and cutover MUST produce source counts, accepted
  counts, transformed counts, rejected or quarantined counts, integrity totals,
  checksums where applicable, and documented discrepancies by entity and tenant.
- **FR-053**: Production cutover MUST require an approved compatibility window,
  restore-tested backup, rehearsal duration, acceptance thresholds, abort criteria,
  rollback or forward-recovery procedure, and post-cutover sign-off.
- **FR-054**: Migration or release interruption MUST NOT allow two owners to apply
  incompatible data changes or allow traffic to use a partially accepted contract.

#### Security, Privacy, and Operations

- **FR-055**: Every read, write, export, administrative action, and background job
  MUST carry an explicit tenant or global scope and enforce least privilege.
- **FR-056**: Commands, callbacks, deep links, imported data, provider input and
  output, URLs, and configuration MUST be treated as untrusted and constrained by
  approved size, format, rate, and destination policies.
- **FR-057**: Any credential-like value ever committed to a source repository MUST
  be treated as compromised, excluded from migration, and independently revoked or
  rotated before its integration is enabled.
- **FR-058**: Personal and conversation data MUST have approved collection,
  retention, export, correction, and deletion rules and MUST be minimized in logs,
  diagnostics, fixtures, and provider exchanges.
- **FR-059**: Audit evidence MUST be tamper-evident and privacy-preserving, and its
  retention MUST be distinct from optional conversation-history retention.
- **FR-060**: Specification, build, test, migration, and runtime tooling MUST remain
  scoped to this project and MUST NOT require host-global installation or alter
  unrelated server services. Current changes MUST be made only in the GitHub
  repository; no server modification, synchronization, container operation,
  migration, or deployment is authorized without new explicit owner permission.
- **FR-061**: Operators MUST be able to distinguish startup, alive, ready, and
  dependency-degraded states without receiving secrets or raw private content.
- **FR-062**: Warmup, weekly grant, cleanup, album, pending moderation, and other
  recurring work MUST be idempotent, bounded, observable, and safe across multiple
  workers.
- **FR-063**: User requests, provider calls, privileged actions, migrations, and
  jobs MUST be correlated through redacted logs, metrics, traces, and actionable
  alerts.
- **FR-064**: Dependency outage, quota exhaustion, overload, shutdown, and restart
  MUST degrade safely, preserve accepted durable work, and avoid duplicate
  user-visible outcomes.
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
  access state, and tenant-specific participation.
- **Tenant / Group**: The isolation boundary for members, administrators, policy,
  configuration, conversation behavior, moderation, tokens, and reporting.
- **Role Assignment**: A current, scoped authorization relationship for regular
  user, group administrator, anonymous administrator, or superadmin behavior.
- **Administrator Group Context**: A private-panel binding between one
  administrator and one currently selected manageable group, including expiry and
  authorization state.
- **Conversation Session**: An owner- and tenant-scoped continuity boundary with
  lifecycle, provider correlation, and eligible new-deployment history. The first
  cutover uses fresh conversational context; no legacy context-bearing reference
  may restore an old conversation.
- **Conversation Record**: A user or assistant exchange retained according to
  approved privacy and history policy.
- **Feedback / Unknown Item**: A response evaluation or unresolved question linked
  to an eligible session, tenant, actor, report state, and audit history.
- **Configuration Value**: A typed value with scope, precedence, revision, actor,
  validation result, and effective period.
- **Ads Policy**: Per-tenant moderation enablement, action, threshold, exemption,
  cooldown, cleanup, and provider-selection rules.
- **Ads Decision**: The explainable classification and policy outcome for one
  logical content item, including exemption, confidence, status, and correlation.
- **Ads Example**: A tenant-scoped labeled example used to improve classification,
  with source, label, actor, and lifecycle.
- **Ads Allowlist Entry**: A tenant-scoped approved user or domain exemption with
  actor and lifecycle.
- **Media Album**: A tenant-scoped logical content item composed of one or more
  message identifiers and a bounded aggregation lifecycle.
- **Pending Moderation Action**: Durable, exclusively owned work with target,
  schedule, attempts, status, recovery state, and terminal outcome.
- **Wallet**: A non-negative balance uniquely owned by one tenant and one user.
- **Weekly Grant**: An idempotent grant eligibility and outcome for one tenant,
  user, and grant period.
- **Ledger Event**: An immutable grant, spend, correction, or reversal with amount,
  cause, deduplication reference, actor when applicable, and time.
- **Deduplication Record**: A scoped identity proving that a logical request, job,
  moderation action, grant, or spend has already been accepted.
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
- **SC-003**: Under the agreed reference workload, at least 99% of accepted user
  requests show progress or a clear acknowledgment within 2 seconds, and at least
  95% reach an answer or safe terminal outcome within 20 seconds, excluding a
  declared upstream outage.
- **SC-004**: In authorization and adversarial testing, 100% of unauthorized
  privileged attempts are denied, and zero tests disclose or modify another
  tenant's configuration, conversation, moderation, token, or audit data.
- **SC-005**: Across the approved moderation corpus, every exempt or
  insufficient-confidence item avoids destructive action, every expected action
  occurs at most once, and repeated delivery creates zero duplicate warnings,
  deletions, or charges.
- **SC-006**: Across concurrent grant and spending tests, wallet balances never
  become negative, each eligible weekly grant occurs at most once, each logical
  spend occurs at most once, and ledger totals reconcile exactly to wallet changes.
- **SC-007**: Every migration rehearsal accounts for 100% of source rows in scope
  as imported, transformed, rejected, quarantined, or excluded; zero secret values
  or legacy conversation histories are promoted, no historical conversational context is
  restored, and all separately approved financial-like totals reconcile without
  unexplained difference.
- **SC-008**: A full backup restoration and cutover rollback drill completes within
  the owner-approved maintenance window and restores all release-critical user,
  tenant, moderation, token, and audit invariants.
- **SC-009**: 100% of user-visible keys used by approved launch journeys are
  complete and format-valid in Persian, English, Arabic, Turkish, and Russian, with
  zero raw keys or developer-only messages visible in acceptance testing.
- **SC-010**: At least 90% of representative users and group administrators
  complete their primary conversation or management journey on the first attempt
  without operator assistance.
- **SC-011**: During interruption and duplicate-delivery tests, accepted durable
  work recovers within 5 minutes of service restoration and produces zero duplicate
  user-visible or ledger effects.
- **SC-012**: Every simulated critical dependency, security, queue-age, migration,
  or reconciliation failure produces an owned, actionable alert within 5 minutes
  without exposing private content or credentials.
- **SC-013**: A release and rollback can each be completed from reviewed
  project-scoped inputs within 30 minutes, without installing host-global tools or
  changing unrelated server services.
- **SC-014**: The approved release workload supports at least twice the measured
  legacy 30-day peak, or an owner-approved equivalent reference workload when
  legacy measurements are unavailable, while meeting SC-003 and producing no lost
  accepted work.

## Assumptions

- The read-only evidence baseline is the `ai_bot6` default branch at commit
  `cdb0efd4b39eaafd1265822350606887fe3b40b3` as inspected on 2026-08-17. Later
  legacy changes are out of baseline until explicitly reviewed and added.
- The rewrite remains a Telegram bot serving private chats, groups, supergroups,
  group topics, callbacks, and administrator private-panel journeys.
- Preserve is the default proposal for observable product behavior; internal
  structure and unsafe deployment behavior are not compatibility contracts.
- The first cutover targets the currently represented legacy deployment and its
  approved tenants, while the resulting product must support independent future
  deployments without source forks or host-global installation.
- Persian, English, Arabic, Turkish, and Russian are launch locales because the
  legacy interface exposes all five selectors. A locale is not considered supported
  until its complete catalog passes acceptance.
- The owner has decided that the first cutover starts with empty conversation
  history and fresh conversational context. Legacy history is intentionally
  excluded without deleting or modifying the old data. BR-004 still proposes
  history/export/clear capabilities for conversations created by the new bot;
  other entity-import decisions remain separate.
  [NEEDS CLARIFICATION: What collection, retention, deletion, and user-notice rules
  apply to conversation content created by the new deployment?] Starting empty
  does not settle these future-history privacy rules.
- Legacy data access will use sanitized snapshots or controlled exports. Live
  production credentials and raw secrets are neither required nor permitted for
  specification and test work.
- External Telegram and AI-answer/classification services remain dependencies, but
  their product-facing failures must follow this specification and no provider's
  internal model becomes a user contract.
- When an undocumented legacy behavior is discovered, it is added to the
  classification ledger rather than inferred as approved.
- Detailed capacity limits, retention durations, and cutover windows will use
  measured legacy evidence where available and explicit owner-approved baselines
  otherwise.
- AdsGuard's token-exhaustion behavior and automatic-deletion quality gates remain
  unapproved. [NEEDS CLARIFICATION: Should the verified legacy token/moderation
  branches be preserved or changed, and what independently measured classification
  quality and false-positive limits permit automatic deletion?] A provider's
  confidence value alone does not settle the acceptable product error rate.
- Recovery targets distinguish deduplicated internal ledger changes from an
  external action whose result is unknown. For example, a reply or warning may
  reach Telegram before a worker stops without recording completion.
  [NEEDS CLARIFICATION: For externally uncertain sends or moderation actions,
  should recovery reconcile first, wait for operator review, or retry under an
  explicitly accepted duplicate-risk policy?] The no-duplicate targets are draft
  targets, not evidence that this failure case has been resolved.
- SC-003, SC-005, SC-008, SC-010, and SC-014 are proposed targets pending an
  owner-approved workload, independently labelled moderation corpus and quality
  limits, maintenance window, participant/sample definition, and measured legacy
  peak or approved substitute. These measurement contracts must be recorded
  before specification approval; no production values have been inferred.

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
- Approved privacy, retention, service-level, maintenance-window, and data-residency
  targets before architecture-plan approval.
