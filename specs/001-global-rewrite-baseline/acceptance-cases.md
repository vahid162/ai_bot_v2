# Additional acceptance-case definitions

Status: Draft, documentation only; no tests implemented or executed. These supplement, not replace, the numbered Given/When/Then scenarios in [spec.md](spec.md). Source/actor/impact trace: [behavior-ledger.md](behavior-ledger.md). Pending owner decisions: [decision-register.md](decision-register.md). Content fixtures: [test-corpus.md](test-corpus.md). Timing, samples and denominators: [measurement-contract.md](measurement-contract.md).

A case with an unresolved D-006–D-013 dependency is not a complete approved test oracle. Expected policy choices must be resolved before claiming ready-for-plan or a passing test. For approved D-001–D-005, D-006's private access policy, D-007's automatic guard mode and D-008's no-action/no-charge rules and D-011's conditional token-continuity intent, their exact scope and limits apply now; remaining D-007 contracts and actual import mappings are not approved. Scope security/privacy requirements remain constraints; legacy unsafe output is not an expected pass. No unsupported Telegram lookup, exactly-once delivery or real deployment identity is assumed.

## Governance

| Case | Given / When / Then contract | Requirements |
| --- | --- | --- |
| AC-GOV-01 — Immutable scope | Given ai_bot6 and the server/checkouts as read-only targets, when this documentation change is reviewed, then the write set contains only approved ai_bot_v2 GitHub documents and no source/server/Docker/migration operations. | FR-001, FR-060 |
| AC-GOV-02 — Discovery accounting | Given every discovered source branch including unsafe or intended-but-broken paths, when the inventory is reviewed, then revision/evidence, actor, proposal, owner, impacts, transition and at least one acceptance case are recorded; uncovered behavior stays unresolved. | FR-002, FR-003, FR-004, FR-005 |
| AC-GOV-03 — Approval gates | Given no explicit whole-spec and later plan approval, when any implementation or dependent planning is proposed, then it remains blocked; individual answers/docs publication do not approve scope, migration or deletion activation. | FR-006 |

## BR-001 — Discovery, identity, menus and health

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-001--discovery-identity-menus-and-health). Existing scenarios: US2.1; US7.1/4; US8.1.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR001-01 — Scope discovery | Given private/group users and each role, when start/help/home and menus are requested in every launch locale, then only applicable capabilities are presented and no privileged data is disclosed. |
| AC-BR001-02 — Menu is not permission | Given a hidden or forged privileged command, when an unauthorized actor invokes it, then current authorization denies it regardless of menu visibility. |
| AC-BR001-03 — Own identity | Given two actors in separate scopes, when whoami is invoked, then each sees only approved fields of their own identity. |
| AC-BR001-04 — Safe health | Given a dependency failure containing a synthetic secret and private marker, when health is viewed, then a localized safe status appears without either marker or provider internals. |

## BR-002 — Private questions, ask and chunked replies

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-002--private-questions-ask-and-chunked-replies). Existing scenarios: US2.1/5/7–10.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR002-01 — Input routes | Given an authorized actor and supported plain/ask/reply/caption input, when submitted once, then one correlated logical response follows the approved route contract and global chat OFF prevents provider calls. |
| AC-BR002-02 — Prompt validation | Given a stale, wrong-owner or command reply, when a question prompt is answered, then it cannot mutate settings or trigger unintended AI work. |
| AC-BR002-03 — Provider failure | Given timeout, malformed output or rate limit, when a request is processed, then progress ends in a safe localized disposition without leaking internals. |
| AC-BR002-04 — Partial reply | Given a multi-chunk reply with an unknown chunk outcome, when recovery or redelivery occurs, then completed/unknown chunks are not replayed and the unresolved disposition is recorded. |

## BR-003 — Group conversation and cleanup

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-003--group-conversation-and-cleanup). Existing scenarios: US2.2–5; US3.2.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR003-01 — Group matrix | Given two groups and topics with disabled/mention/all and administrator-only settings, when addressed/unaddressed member/admin inputs arrive, then only the approved eligible combinations call AI and no context crosses scope. |
| AC-BR003-02 — Forward eligibility | Given manual and automatic discussion forwards, when each reaches group chat, then the approved D-010 eligibility rule applies consistently across plain and ask routes. |
| AC-BR003-03 — Frequency boundary | Given a configured group/topic response gap, when eligible inputs arrive just before/at/after the boundary, then the documented throttle applies only in that scope. |
| AC-BR003-04 — Cleanup targets | Given cleanup enabled/disabled and bot/user message targets, when cleanup runs, then only D-007-approved targets/actions occur; unknown deletion results are held, not automatically repeated. |

## BR-004 — Sessions, history, clear and export

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-004--sessions-history-clear-and-export). Existing scenarios: US2.6/8–10; US5.1/6.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR004-01 — Owner export | Given consenting/nonconsenting owners in two groups/topics, when export/clear is attempted by owners and other users/admins, then only eligible owner text is accessible or removed and empty results are explicit. |
| AC-BR004-02 — Consent boundary | Given storage off, later opt-in and later opt-out, when exchanges occur, then no earlier text is backfilled, no unconsented history persists, and opt-out stops future storage without pretending to clear retained history. |
| AC-BR004-03 — Lifetime boundary | Given stored messages and earlier clear, when original creation plus 30×24h UTC is reached or restored copies are read, then expired/cleared text cannot reappear in history, context, export, feedback or diagnostics. |
| AC-BR004-04 — Fresh context | Given excluded legacy history and context-bearing references, when first cutover/start/clear/ask/private routes run, then no old conversation is restored and D-010-approved reset semantics are consistent. |

## BR-005 — Feedback, unknown reporting and review

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-005--feedback-unknown-reporting-and-review). Existing scenarios: US2.7/10; US3.3.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR005-01 — Feedback identity | Given concurrent repeated or forged callbacks, when voting occurs, then the approved vote rule is enforced for the actor/answer and unrelated users cannot overwrite it. |
| AC-BR005-02 — Unknown journey | Given an unknown answer, when reporting and viewing guidance through each approved input route, then the registered journey has an explicit outcome rather than an unverified helper path. |
| AC-BR005-03 — Scoped report | Given a report ID from another user/tenant, when a report or review is attempted, then protected data is neither disclosed nor modified and repeated valid reports remain idempotent. |
| AC-BR005-04 — Privacy review | Given unconsented/expired/cleared text, when feedback or unknown review runs, then it retains no raw text and admins see only currently authorized records. |

## BR-006 — Direct-message access and user listing

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-006--direct-message-access-and-user-listing). Existing scenarios: US2.4; US3.3.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR006-01 — Access truth table | Given explicit private denial and all eight global-on/off × environment-allow yes/no × managed-allow yes/no combinations, when ordinary private/ask/reply or not-yet-executed queued provider/answer work dispatches, then denial wins in every combination. If denial is absent, no allowance means deny and any approved allowance remains subject to ordinary feature/access gates. Unblock alone grants no allowance. Restart/ordinary allow/global changes cannot clear denial; earlier unknown Telegram effects stay held. |
| AC-BR006-02 — Global authorization | Given group/admin/forged/stale-superadmin actors, when global allow/block/unblock/dm actions are attempted, then only current authorized superadmin scope can change the explicit private denial. Unauthorized attempts produce no mutation and are audited; group role or a visible menu grants no global authority. |
| AC-BR006-03 — Access targets | Given malformed/cross-scope targets, repeated authorized block/unblock and a prior failed write, when access is changed, then validated idempotent audited outcomes preserve unrelated users, failed writes are not reported as success, and only an explicit authorized unblock clears denial. A normal allow is not unblock, and a missing historical allow row is not inferred block/import permission. Exact interaction naming/migration oracle remains D-010/D-011. |
| AC-BR006-04 — User list | Given empty and populated approved user-list fields, when an authorized list is requested, then fields render safely and no wrong-scope/raw-content values are returned. |

## BR-007 — Private management and bound prompts

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-007--private-management-and-bound-prompts). Existing scenarios: US3.1/2/4.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR007-01 — Group binding | Given valid/invalid deep links and two administrators/groups, when management is opened, then only manageable groups are selectable and active scope is explicit per actor. |
| AC-BR007-02 — Revoked role | Given a previously authorized panel whose actor loses the role, when read or mutation is attempted, then it fails safely without data disclosure or mutation. |
| AC-BR007-03 — Group switch race | Given a prompt for group A and later selection of B, when the old prompt is answered, then it cannot write B or another actor's state. |
| AC-BR007-04 — Prompt integrity | Given stale, wrong-owner, wrong-message or unknown-field replies, when editing is attempted, then no setting is changed and it cannot fall through as an unintended AI question. |

## BR-008 — Roles, superadmins and privileged audit

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-008--roles-superadmins-and-privileged-audit). Existing scenarios: US3.3/4; US8.2.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR008-01 — Role matrix | Given all actor roles including removed and anonymous admins, when private/global/group actions are attempted, then authority is current and limited to the approved scope. |
| AC-BR008-02 — Spoofed authority | Given forwarded/bot/sender-chat or forged superadmin input, when a privileged path is invoked, then it is denied with a redacted audit result. |
| AC-BR008-03 — Membership mutation | Given current authorized superadmin membership changes and repeated targets, when applied, then the approved membership and bootstrap precedence yields an explicit result. |
| AC-BR008-04 — Audit failure | Given accepted/denied privileged attempts and an unavailable audit sink, when processed, then D-009's approved failure disposition is enforced without raw text or secret leakage. |

## BR-009 — Scoped settings, language and menus

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-009--scoped-settings-language-and-menus). Existing scenarios: US3.2/4; US7.2/3.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR009-01 — Setting matrix | Given every approved setting at invalid/minimum/maximum boundaries in two groups, when read/written through command and panel paths, then typed bounds and current scope are enforced consistently. |
| AC-BR009-02 — Language callbacks | Given private/group language callbacks and revoked/forged roles, when invoked, then only authorized scope changes and locale display remains safe. |
| AC-BR009-03 — Mode versus activation | Given delete mode selected but no valid owner activation/evaluation, when content arrives or pending work recovers, then no automatic user-content deletion is attempted. |
| AC-BR009-04 — Persistence failure | Given a failed configuration write, when the UI responds, then it cannot claim a successful save or expose a new effective value. |

## BR-010 — AdsGuard classification and eligibility

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-010--adsguard-classification-and-eligibility). Existing scenarios: US4.1–6/10.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR010-01 — Eligibility matrix | Given roles, user/domain/reply/contact exemptions and two tenants, when content is evaluated, then only approved eligible paths reach classification/enforcement and scope cannot leak. |
| AC-BR010-02 — Output confidence | Given null/malformed/missing/below/equal/above-threshold classification output, when policy runs, then D-007-approved boundary logic applies and missing/untrusted evidence cannot authorize destruction. |
| AC-BR010-03 — Reachable content | Given fresh and edited text/captions, manual/automatic forwards and each media kind, when routed, then each D-010-approved form has a tested reachable outcome rather than relying on an internal branch alone. |
| AC-BR010-04 — Album ordering | Given reordered, duplicate and late captions for one album, when aggregation finishes, then one scoped logical classification identity and member outcomes are preserved. |

## BR-011 — Modes, forward/caption guards, profiles and mute

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-011--modes-forwardcaption-guards-profiles-and-mute). Existing scenarios: US4.1/3/7–10; US6.4.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR011-01 — Mode and wallet | Given modes off/none/warn/delete, token switches and sufficient/exhausted wallets for singles/albums, when a trusted eligible ad is processed, then D-002 governs exhaustion and D-008's no-action branch has zero AdsGuard spend/debit or successful-spend event. Duplicate/edit/queued/recovered work and current-mode checks cannot charge a no-action evaluation. No invalid spend is successful; warning/deletion charges and other economic rules remain pending. |
| AC-BR011-02 — Forward and caption guards | Given eligible disallowed forwards or captionless singles/albums under each mode/profile, when automatic guard/grace work evaluates or executes, then current disabled/off and no-action states have no moderation action, warning never deletes user content, and deletion mode only permits deletion with current eligibility/authorization/exemptions and D-005 quality plus separate owner activation. Profiles and queued/recovered jobs cannot bypass a change away from deletion mode; unknown prior results stay held under D-003. Exact eligibility/grace/notice oracles remain pending in D-007. |
| AC-BR011-03 — Caption race | Given caption edit/reply before/at/after grace and concurrent execution, when cancel/recovery occurs, then at most one authorized effect follows current mode and known/unknown result separation; a queued deletion cannot execute after switching to off/none/warn. The exact grace/edit/cancel boundary oracle remains pending; this approved mode rule does not invent it. |
| AC-BR011-04 — Manual mute | Given forged/stale/exempt/authorized mute targets including an album, when invoked, then only D-007-approved associated actions occur and partial/unknown external effects are not repeated. |
| AC-BR011-05 — Deletion gate | Given absent/failed/out-of-scope/revoked evidence or activation for new/queued/recovered work, when automatic deletion would run, then zero attempts occur; only matching approved evidence and separate owner activation can permit it. |

## BR-012 — Pending moderation and album recovery

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-012--pending-moderation-and-album-recovery). Existing scenarios: US4.3/4/10; US8.2/4.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR012-01 — Crash disposition | Given interruption before send/after confirmed completion/confirmed non-execution/unknown execution, when restarted, then only confirmed non-execution permits bounded authorized retry; unknown results are durably held. |
| AC-BR012-02 — Partial album | Given completed, not-executed and unknown member results, when album work recovers, then no completed or unknown member action is repeated and the logical outcome remains accurate. |
| AC-BR012-03 — Upsert state | Given cancelled/completed/unknown work and duplicate inputs, when upsert or recurring recovery occurs, then no terminal state or unknown hold is silently reset. |
| AC-BR012-04 — Policy recheck | Given changed mode/enablement/role/exemption/deletion gate, when pending work runs, then current safeguards apply and outdated permission cannot authorize destruction. No-action AdsGuard before an uncommitted debit also prevents spending under stale prior-mode admission; no automatic refund of already committed debit or repeat of unknown Telegram action follows. |
| AC-BR012-05 — Preserved identifiers | Given cache cleanup/restart, when unresolved album work remains, then required non-content identifiers remain available or the action is safely held; no new spend/refund is inferred. |

## BR-013 — Examples, allowlists and diagnostics

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-013--examples-allowlists-and-diagnostics). Existing scenarios: US3.2; US4.2/5.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR013-01 — Tool routes | Given every registered and advertised example/stats/simulate/probe alias, when invoked with valid/invalid arguments, then D-010-approved aliases share correct parsing and fresh authorization. |
| AC-BR013-02 — Tool isolation | Given an unauthorized or other-tenant actor, when examples/allowlists/stats/debug are requested, then no data is disclosed or changed. |
| AC-BR013-03 — Example mutations | Given positive/negative labels, caps, duplicate/invalid domains/users and clear confirmation, when tools run, then only approved scoped changes occur with audit and content policy. |
| AC-BR013-04 — Diagnostic effects | Given a probe or simulation including classifier errors, when run, then no runtime warning/deletion/mute, token debit or policy mutation occurs; only approved diagnostic outputs/ancillary metadata are allowed. |
| AC-BR013-05 — Debug control | Given group users and private superadmins, when debug control is invoked, then only approved private-global authority succeeds and outputs are redacted. |

## BR-014 — Wallet balance, weekly grants and carry

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-014--wallet-balance-weekly-grants-and-carry). Existing scenarios: US6.1/2/5.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR014-01 — Balance scope | Given balance aliases in group/private/no-active-group contexts, when queried, then only the caller's approved tenant wallet is shown with an explicit missing-context outcome. |
| AC-BR014-02 — Grant race | Given lazy/scheduled/concurrent triggers around the D-008-approved period boundary, when grants execute, then at most one eligible grant event changes that tenant/user/period. |
| AC-BR014-03 — Carry and eligibility | Given balances at/above/below the approved cap and already-granted users, when grant/spend/grant repeats, then no second grant occurs and D-008's eligibility/cap-reduction policy is explicit. |
| AC-BR014-04 — Switches and next grant | Given global/group enablement combinations, when lookup or grants run, then approved effective state and localized next-window information are correct without unintended grant/debit. |

## BR-015 — Token spending and ledger outcomes

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-015--token-spending-and-ledger-outcomes). Existing scenarios: US6.3–5; US4.7–9.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR015-01 — Spend concurrency | Given repeated concurrent logical singles/albums, when AdsGuard's effective mode is no-action before an uncommitted debit, then zero attributable token spend/debit or successful-spend events occur with any balance/switches, including duplicate/edit, restart and queued/recovered processing. Current mode prevents stale charging and a recorded no-action outcome is not retro-charged. Independent authorized grants may change balance separately; other approved consuming journeys still have at most one matching debit/ledger event. Warning/deletion/later-distinct-journey oracles remain pending. |
| AC-BR015-02 — Failed spend replay | Given zero balance and an insufficient attempt, when redelivered before or after a later grant, then the original attempt never becomes a fictitious success or unapproved retroactive charge. |
| AC-BR015-03 — Long replay | Given an old logical spend after cleanup/restart/restore, when redelivered, then the approved replay contract prevents a second charge and identifies the original outcome. |
| AC-BR015-04 — Scoped keys | Given equal logical keys in different tenants/users, when spent, then each approved wallet outcome remains isolated. |
| AC-BR015-05 — Corrections | Given failed/unknown external outcomes and a proposed correction/reversal, when reconciled, then only separately authorized events affect balances and ledger totals exactly match all changes. |

## BR-016 — Configuration precedence and startup seeding

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-016--configuration-precedence-and-startup-seeding). Existing scenarios: US7.2/3; US3.2/4.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR016-01 — Precedence | Given four candidate values for one setting in two tenants, when candidates are successively removed, then FR-042's documented precedence holds without affecting the other tenant. |
| AC-BR016-02 — Invalid setting | Given malformed/out-of-range input, when submitted, then the prior valid value remains unchanged and a redacted failure outcome is recorded. |
| AC-BR016-03 — Restart seeding | Given existing approved managed values, when runtime restarts with different deployment defaults, then approved administrator choices are not silently overwritten. |

## BR-017 — Localization coverage and fallback

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-017--localization-coverage-and-fallback). Existing scenarios: US7.1/4.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR017-01 — Five-locale journeys | Given the approved message/key manifest for every journey, when each runs in fa/en/ar/tr/ru, then placeholders/plurals/buttons/errors/RTL-LTR are complete and safe. |
| AC-BR017-02 — Broken catalog | Given missing/corrupt/stale format entries, when readiness and output are checked, then release fails and no raw key/developer fallback is shown. |
| AC-BR017-03 — Locale isolation | Given unsupported regional hints and two tenants, when locale resolves, then D-010-approved fallback applies without changing another tenant's preference. |

## BR-018 — Operations, health and recurring work

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-018--operations-health-and-recurring-work). Existing scenarios: US8.1/2/4; US6.1.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR018-01 — Health states | Given startup/alive/unready/degraded states and synthetic secret/private markers, when health/alerts are emitted, then distinctions are correct and markers absent. |
| AC-BR018-02 — Sink failure | Given logging/metrics/error-sink outage or backpressure, when work executes, then D-009-approved safe disposition preserves privacy and accepted work. |
| AC-BR018-03 — Recurring concurrency | Given two workers/restarts triggering the same recurring business item, when processed, then business effects are at most once and work is bounded/observable. |
| AC-BR018-04 — Recovery alerts | Given unknown reply/warning/delete and restored service, when recovery runs, then SC-011's disposition and SC-012's owned alert meet their clocks without automatic repeat. |

## BR-019 — Candidate imports and schema drift

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-019--candidate-imports-and-schema-drift). Existing scenarios: US5.1–6; US1.1–3.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR019-01 — Schema conflict | Given unknown/conflicting source schema or keys, when import is proposed, then it is rejected/quarantined pending approved mapping rather than guessed. |
| AC-BR019-02 — Ownership conflict | Given missing/conflicting owner/tenant/role relationships, when mapped, then no ambiguous row is promoted or treated as current authority. |
| AC-BR019-03 — Complete accounting | Given every candidate source row, when rehearsal is authorized, then exactly one disposition and reason is counted per source identity with independent totals. |
| AC-BR019-04 — Excluded history | Given legacy history/provider references and source checksum, when cutover is rehearsed, then no old text/context is restored and legacy source checksums remain unchanged. |
| AC-BR019-05 — Business reconciliation | Given D-011 selected validated wallet/ledger continuity plus separately approved actual sanitized schema/identity mappings, necessary linked grant/dedupe evidence and actual opening-baseline evidence, when a future authorized import is repeated and lazy/scheduled grants or replay follow, then source balances and mapped associated ledger records reconcile exactly per tenant/user, without a second grant/debit or false successful-spend outcome from mere row existence. Missing/conflicting evidence is rejected/quarantined and blocks promotion/cutover; never fabricate balances or balancing events. Historic debits are not rewritten/refunded. Other entity imports, including allowlist/pending work, still require separate selection/mapping; unknown external effects remain held and history/context/secrets are excluded. No import or tests run now. |

## BR-020 — Migration ownership

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-020--migration-ownership). Existing scenarios: US5.4/5; US8.3.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR020-01 — Competing migration | Given two attempted incompatible migration owners, when a future authorized rehearsal runs, then only one approved owner can mutate the accepted contract. |
| AC-BR020-02 — No startup upgrade | Given ordinary runtime startup, when launched, then it cannot implicitly mutate schema or start a migration. |
| AC-BR020-03 — Failed compatibility | Given failed migration/reconciliation, when traffic promotion is requested, then it is blocked with an owned abort disposition. |

## BR-021 — Unsafe and nonportable release inputs

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-021--unsafe-and-nonportable-release-inputs). Existing scenarios: US5.3; US8.1/3.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR021-01 — Secret exclusion | Given synthetic credential-like inputs, when source/import/release review runs, then none are promoted or printed and only location/category is reported. |
| AC-BR021-02 — Missing secret | Given a missing required integration secret, when activation is attempted, then it fails safely without revealing values. |
| AC-BR021-03 — Project isolation | Given two independently configured project deployments, when tested after approval, then names/storage/networks/tooling do not collide or alter unrelated services. |
| AC-BR021-04 — Repeatable release | Given reviewed pinned inputs, when release and rollback are rehearsed, then both reproduce their reviewed state within SC-013's clock. |

## BR-022 — Mixed internal responsibilities

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-022--mixed-internal-responsibilities). Existing scenarios: US1.4; all approved journeys.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR022-01 — Outcome equivalence | Given all approved scenario contracts, when internal boundaries are later changed, then observable authorized outcomes and data/security invariants remain equivalent. |
| AC-BR022-02 — Change gate | Given a newly discovered observable difference, when reviewed, then it is recorded and approval-gated before dependent work rather than dismissed as refactoring. |

## BR-023 — Generated artifacts tracked as source

Actor, evidence and impacts: [ledger record](behavior-ledger.md#br-023--generated-artifacts-tracked-as-source). Existing scenarios: US1.4; US8.3.

| Case | Given / When / Then contract |
| --- | --- |
| AC-BR023-01 — Clean source | Given a proposed reviewed source tree, when inspected, then generated bytecode/cache/local runtime outputs are excluded from reviewed inputs. |
| AC-BR023-02 — Recreated outputs | Given only approved source and resolved release inputs, when built after authorization, then required generated outputs are recreated without importing legacy bytecode. |

## Functional requirement coverage index

Each of the 65 existing requirements has explicit supplementary case pointers. These document traceability, not implemented coverage, complete owner-approved oracles or passing tests. Cases supplement the original user-story scenarios; multiple cases may be required together.

| Requirement | Supplementary acceptance cases |
| --- | --- |
| FR-001 | AC-GOV-01 |
| FR-002 | AC-GOV-02 |
| FR-003 | AC-GOV-02, AC-BR022-02 |
| FR-004 | AC-GOV-02 |
| FR-005 | AC-GOV-02, AC-BR022-02 |
| FR-006 | AC-GOV-03 |
| FR-007 | AC-BR001-01, AC-BR001-03, AC-BR001-04 |
| FR-008 | AC-BR006-01, AC-BR006-02 |
| FR-009 | AC-BR002-01, AC-BR002-04 |
| FR-010 | AC-BR003-01, AC-BR003-03 |
| FR-011 | AC-BR004-02, AC-BR004-03, AC-BR003-01 |
| FR-012 | AC-BR004-01, AC-BR004-03, AC-BR004-04 |
| FR-013 | AC-BR002-03, AC-BR001-01 |
| FR-014 | AC-BR002-03, AC-BR002-04 |
| FR-015 | AC-BR005-01, AC-BR005-02, AC-BR005-03, AC-BR005-04 |
| FR-016 | AC-BR008-01, AC-BR008-02 |
| FR-017 | AC-BR007-02, AC-BR007-04, AC-BR008-01, AC-BR012-04 |
| FR-018 | AC-BR007-01 |
| FR-019 | AC-BR007-03, AC-BR007-04 |
| FR-020 | AC-BR009-01, AC-BR016-01, AC-BR016-02 |
| FR-021 | AC-BR006-02, AC-BR008-03, AC-BR013-05 |
| FR-022 | AC-BR001-02, AC-BR009-02 |
| FR-023 | AC-BR008-04, AC-BR006-02, AC-BR009-04 |
| FR-024 | AC-BR011-01, AC-BR011-02, AC-BR012-04, AC-BR009-03 |
| FR-025 | AC-BR010-01, AC-BR010-03 |
| FR-026 | AC-BR010-04, AC-BR012-02 |
| FR-027 | AC-BR010-02, AC-BR011-05 |
| FR-028 | AC-BR011-01, AC-BR011-02, AC-BR011-03, AC-BR011-04, AC-BR003-04 |
| FR-029 | AC-BR012-01, AC-BR012-03, AC-BR012-04 |
| FR-030 | AC-BR010-04, AC-BR012-02, AC-BR012-05 |
| FR-031 | AC-BR013-02, AC-BR013-03 |
| FR-032 | AC-BR013-01, AC-BR013-03 |
| FR-033 | AC-BR013-04 |
| FR-034 | AC-BR013-02, AC-BR013-05, AC-BR005-04 |
| FR-035 | AC-BR014-04 |
| FR-036 | AC-BR014-01, AC-BR015-04 |
| FR-037 | AC-BR014-02, AC-BR014-03 |
| FR-038 | AC-BR014-03 |
| FR-039 | AC-BR011-01, AC-BR015-01, AC-BR015-03, AC-BR012-04 |
| FR-040 | AC-BR015-01, AC-BR015-02, AC-BR015-05, AC-BR014-02 |
| FR-041 | AC-BR014-01, AC-BR014-04 |
| FR-042 | AC-BR016-01, AC-BR016-03 |
| FR-043 | AC-BR009-01, AC-BR016-02 |
| FR-044 | AC-BR009-01, AC-BR009-04, AC-BR016-03 |
| FR-045 | AC-BR017-01 |
| FR-046 | AC-BR017-02 |
| FR-047 | AC-BR017-01, AC-BR017-03 |
| FR-048 | AC-BR019-01, AC-BR019-02 |
| FR-049 | AC-BR019-03, AC-BR019-04, AC-BR019-05 |
| FR-050 | AC-BR019-01, AC-BR019-02, AC-BR019-05 |
| FR-051 | AC-BR021-01, AC-BR023-01, AC-BR019-04 |
| FR-052 | AC-BR019-03, AC-BR019-05 |
| FR-053 | AC-BR020-03, AC-BR019-05 |
| FR-054 | AC-BR020-01, AC-BR020-03, AC-BR012-04 |
| FR-055 | AC-BR004-01, AC-BR005-03, AC-BR007-02, AC-BR013-02, AC-BR015-04, AC-BR019-02 |
| FR-056 | AC-BR007-04, AC-BR009-01, AC-BR010-02, AC-BR017-01, AC-BR021-01 |
| FR-057 | AC-BR021-01, AC-BR021-02 |
| FR-058 | AC-BR004-02, AC-BR004-03, AC-BR005-04, AC-BR019-04 |
| FR-059 | AC-BR008-04, AC-BR005-04 |
| FR-060 | AC-GOV-01, AC-BR021-03 |
| FR-061 | AC-BR001-04, AC-BR018-01 |
| FR-062 | AC-BR012-03, AC-BR012-05, AC-BR014-02, AC-BR015-03, AC-BR018-03 |
| FR-063 | AC-BR018-01, AC-BR018-04, AC-BR008-04 |
| FR-064 | AC-BR002-04, AC-BR012-01, AC-BR012-02, AC-BR012-04, AC-BR015-05, AC-BR018-02 |
| FR-065 | AC-BR020-02, AC-BR021-03, AC-BR021-04, AC-BR023-02 |

## Required future result record

Each executed case must identify specification/case/corpus revision, approved decision oracle, actor/scope, declared setup, fault injection, expected/observed effects, timestamps, ledger reconciliation where applicable, redacted evidence and reviewer. Mark PASS/FAIL/BLOCKED/NOT RUN explicitly; unknown external outcome is not PASS for confirmed delivery. A pending oracle is BLOCKED, never guessed. Cross-scope and privacy cases use synthetic markers. P1 cutover coverage includes all relevant US1–US5 cases and their critical governance/security/recovery dependencies, even if a dependency sits under a P2 family. Evaluation and deployment require later explicit authorization.
