# Consolidated recommended contracts

Status: SELECTED RECOMMENDATIONS FOR OWNER REVIEW, 2026-10-11. This supplements [spec.md](spec.md). The owner delegated remaining recommendation choices and requested a consolidated review draft. These are explicit agent-selected policy definitions, not invented question/answer transcripts, verified infrastructure facts or approval of the entire specification/plan.

Accepted D-001–D-005 and later explicit access/mode/no-charge/token-continuity answers remain unchanged. This document resolves remaining **working policy choices** D-006–D-013; evidence and approval gates below remain real dependencies. No application code or test implementation/execution, server access/change, source modification, migration, deployment or deletion activation is authorized.

## D-007 — Moderation, guards and cleanup

### Configuration and eligibility

- New installations default AdsGuard OFF; enabling without an explicit action uses none. Threshold defaults 0.90, finite numeric [0,1], with AD qualifying at score >= threshold. Only structured AD/NOT_AD plus a finite numeric score is valid; boolean/string/missing/out-of-range/NaN/infinite scores or unsupported labels become UNKNOWN. UNKNOWN cannot authorize destruction or charging.
- Classification gap: integer 0–3600 seconds, default 2 per tenant. Few-shot selection: integer 0–50, default 10, within approved example provenance/lifecycle. Throttling records a safe disposition, never pretends classification completed.
- Supported classification input: attributable fresh/edited text and captions on photo/video/document/animation, including manually forwarded text/captions. No binary download, OCR, image understanding, transcription or URL retrieval is selected. Bots, service events and verified automatic discussion-channel posts do not become ads merely by origin.
- Current verified group owners/admins and validated tenant user allowlists are exempt. Missing actor/scope/role/exemption evidence prevents destruction and debit; a failed lookup is not proof of non-exemption.
- Domain exemption requires **all** extracted external hosts to match approved tenant hosts after safe normalization; default exact hostname match, never substring matching or implicit subdomain/wildcard inheritance. Normalize case/valid international host representation, reject malformed hosts, and do not fetch URLs. Mixed allowed/unallowed hosts are not exempt.
- Short-reply/address exemption defaults ON only for known same-scope replies to this bot or exact bot addressing, remaining text <=160 Unicode scalars, with no external URL/email/phone/contact marker. Contact-bearing replies are classified by default; neither contact presence nor a question mark is independently an ad or exemption.
- Entity-forward restriction defaults OFF. Only known chat/channel-origin metadata is eligible for that structural rule; user/hidden-user or absent origin is not guessed into an entity-forward. Other semantic classification still follows eligibility.
- Caption guard defaults OFF, supported caption media only. Minimum defaults 10 words; word mode accepts 0–100 Unicode letter/number runs with attached combining marks. Character mode is an explicit alternate 0–1024 Unicode scalars, default minimum 10 when newly selected. Punctuation/emoji alone is not a word. Changing metrics previews the new interpretation and requires confirmation, never silently converts imported values.
- Caption grace defaults 300 seconds; forward-caption grace 120 seconds; each integer 0–3600. Anchor deadlines to original logical content creation, not edit/retry/restart. Reply-as-caption defaults OFF; when enabled, accept only the original author's same-topic/tenant reply to the precise media or known bot reminder.
- Named preset previews: free = enabled/none/guards OFF; medium = enabled/warn/caption guard ON/entity-forward restriction OFF; strict = enabled/warn/both guards ON. Other valid values remain unchanged; safe defaults fill only absent values. Show exact changed values and reject concurrent revision changes rather than overwrite a newer setting. No preset selects deletion or supplies activation.

### Action and grace contract

A forward/caption violation alone is not advertising evidence and never independently authorizes user deletion. Preserve opt-in reminder/grace utility, but any user deletion additionally requires trusted eligible AD evidence, current delete mode, permissions/exemptions and D-005 evaluation plus **separate owner activation**. Captionless binary content without approved text evidence cannot qualify for deletion. This intentionally changes legacy pre-mode/structural deletion.

Off/none produces no moderation notice. Warn creates at most one localized warning/reminder per logical item/album. Delete-mode structural reminders may explain the deadline but lack of correction alone is not deletion permission; a blocked delete never silently becomes an unapproved warning/mute. Notices use reason/deadline/guidance, not copied message text, phone numbers or raw provider diagnostics. There is no separate unsolicited exhaustion notice; balance guidance may fit inside the single warning. Explicit balance lookup remains available.

A valid correction accepted before the deadline cancels the structural violation; at equality a previously accepted correction wins the execution recheck. A correction accepted after the deadline but before action authorization also cancels stale structural work. Cancellation of a structural violation does not prove an advertising message became legitimate or undo an earlier unknown external effect. All effects recheck latest content/current policy. An already known bot warning can be updated instead of sending another; unknown sends are never repeated.

Albums have one logical outcome/charge identity and separately recorded member effects. Known legitimate or unresolved member evidence must not be hidden by an AD caption or whole-album label. No destructive whole-album action when membership/eligibility is uncertain; only members covered by the independently approved action oracle and trusted eligible evidence can be considered, under every normal gate. Incomplete/oversized albums have no destructive action. Candidate evaluation scores both logical items and legitimate-member deletion proposals.

### Manual and housekeeping actions

Manual mute is distinct explicit confirmed group administration: default 24 hours, integer 1–168; verify current actor/target/bot permissions and exempt targets at execution. It has **no hidden original-message, album or caption-reply deletion**, irrespective of AdsGuard mode. Unknown mute results also follow NO_REPLAY review.

Original-user chat/command auto-cleanup is retired from the initial rewrite scope; it must not delete member messages. Bot-owned housekeeping defaults OFF; explicitly enabled delay 30–3600 seconds, only positively identified bot-owned ephemeral messages. Warn-success cleanup settings can target bot-owned notices only, never escalate into user deletion/mute. Cleanup never clears history/ledger or retries unknown external effects.

Trace: BR-003/010/011/012; FR-024–030/043/044/064; AC-BR003-04, AC-BR010-01–04, AC-BR011-01–05, AC-BR012-04; STR-03/04/08. Communicate structural-only deletion removal, preset preview and retired hidden/user cleanup before a later authorized rollout.

## D-008 — Token economics and replay

| Journey | Selected token charge |
| --- | --- |
| Off/none, disabled tokens, non-ad/exempt/UNKNOWN | 0 |
| Structural reminder, manual mute, bot cleanup, diagnostics or AI chat | 0 |
| Trusted eligible advertisement in warn | At most 1, only after confirmed advertising warning/update and current debit conditions |
| Delete | 0; quality/activation safeguards still required |
| Insufficient balance or unknown warning delivery | 0; warn does not escalate to delete |

Both global and tenant token enablement are required for grants/debits; default both OFF for new installation. Existing balance lookup does not accidentally grant while disabled. Weekly grant is +1 per current attributable non-bot member/tenant, once in [Monday 00:00 UTC,next Monday 00:00 UTC). Current membership/access must be verified; no missed-week backfill. Lazy/scheduled triggers share a settled period outcome. Carry defaults 1, integer 0–100. First eligible period check either grants +1 or settles SKIPPED_CAP; spending later does not reopen a skipped grant. Lowering a cap or importing a valid above-cap balance never burns/clamps existing tokens. Rejoin/restart/enablement toggles do not reset settled period evidence.

Warn debit requires trustworthy confirmation of the AD warning/update, current mode still warn, both switches ON, current wallet/actor eligibility and atomic balance >=1. Current none before an uncommitted debit means zero charge even if warning was delivered. Concurrent balance exhaustion can yield a confirmed free warning, never fictitious payment/negative balance. Warning send confirmation and internal debit confirmation are separate. Never repeat a warning to collect payment. Unknown delivery is held with zero automatic debit/refund; later evidence can reconcile delivery but cannot retro-charge a settled no-charge outcome.

Paid identity is active source epoch + tenant + original author + original message ID, or media-group ID for an album. Edits, caption replies, member ordering and mode/config changes do not create a new paid identity. New authored posts have distinct identities. Persist actual outcome (DEBIT_CONFIRMED, NO_ACTION, INSUFFICIENT, DISABLED, EXEMPT, NON_AD, GUARD_ONLY, UNKNOWN_HELD); never turn an insufficient/none replay into a successful paid journey after a grant.

New automatic journeys require trusted original creation age <=24 hours and active source epoch. Edits do not refresh age. Prior epochs, stale/missing creation evidence and old replay have no new effects; known duplicates return recorded outcomes. Recovery uses the admitted identity, never a fresh paid identity, and current permission/age gates. Closed grant periods cannot be reopened. Compact outcome expiry is allowed only when demonstrated original-age/epoch barriers still prevent regrant/recharge/unknown replay; otherwise purge and affected promotion are blocked. Archival is not execution proof.

No automatic refunds for mode changes, timeout/unknown effects or historical discovery. Current superadmin may explicitly confirm a correction against a proven erroneous tenant/user event, with exact justified amount, linked immutable compensating event, redacted reason/audit and nonnegative result. No discretionary top-up/sales feature is selected; missing evidence blocks correction. Imported events are not rewritten; a reconciled non-event checkpoint is not a fabricated balancing grant/spend.

Trace: BR-014/015/019; FR-035–041/049/050/052/053/062/064; AC-BR014-01–04, AC-BR015-01–05, AC-BR011-01, AC-BR019-05; STR-07; SC-005/006/007. Disclose confirmed-warning-only charges, free ambiguous/raced warnings, preserved above-cap balances and no backfilled grants.

## D-009 — Mandatory evidence, outages and review

Record durable redacted attempt/admission evidence **before** privileged authorization evaluation or dispatch. Required audit/business-outcome/ledger tracking failure blocks affected new admission and external dispatch; return bounded generic unavailable guidance, not a fabricated audited denial or successful save. Mark the affected capability unready and preserve admitted durable work. Every actually processed accepted/denied privileged attempt must have required audit evidence; network arrival alone is not admission. There is no promise of durable recording of all arriving packets while every store is down, and this limitation never permits unaudited privileged processing or false 100% coverage.

Optional metrics/log-export/error-reporting failure alone permits only independently safe work in a degraded state: authorization, privacy/provider policy and required outcome tracking must still work. No raw/unbounded buffer, disabled redaction, assumed persistence or unknown-action repeat. A previously evidenced attempt with failed outcome persistence is reconciled as unknown until trustworthy available evidence resolves it; do not reconstruct gaps as observed facts.

Owner designates a current scoped superadmin incident/review owner and replacement. Group admins see moderation-review metadata only for currently manageable groups; not another user's private conversation/provider payload. Evidence is logical identity, scope/action/target identifiers, attempt/result times, policy/authority revision, non-content evidence reference, reason, owner and disposition. Assignment is neither replay/refund authority. Queue/dedupe rows, timeouts, provider success or absent messages in an incomplete view do not prove Telegram execution/non-execution. No generic result-lookup facility is assumed.

After 30 days, an unresolved external case moves to archived unresolved/NO_REPLAY, never inferred success/failure/refund. Compact safety outcome evidence is retained 365 days; purge requires independent age/epoch barriers against reopening. Repeated updates, UI edits, imports and job claims cannot revive it.

SC-012 retains the <=300-second owned actionable alert-production target. Distinguish durably produced/independently evidenced alerts from notification delivery; unavailable channels report NOT_DELIVERED, never PASS by assumption. Full required-store loss fails affected readiness/evidence verification. A plan must demonstrate alert/evidence coverage; any required target it cannot meet remains FAIL or requires a separately reviewed amendment, not an implicit waiver.

Trace: BR-005/007/008/009/012/018; FR-017/023/055/059/061/063/064; AC-BR008-04, AC-BR018-02/04, AC-BR005-03/04, AC-BR012-01/03/05.

## D-010 — Reachable interaction contract

- One pipeline for authorized plain private text, /ask arguments, eligible reply text/caption and matching /ask ForceReply. Arguments win over reply content; empty questions open a five-minute bound prompt, no provider request. Current access/feature/privacy/failure/feedback gates are identical. Without history opt-in use current exchange only; with it, use only live own same-tenant/topic/current-session entries. Borrowed visible reply content may be transient input but cannot be stored as caller-owned history without the actual owner's permission.
- Private /start resets conversational context and opens home, not history/consent/access/ledger; group /start reports readiness. /start grp_<chat_id> verifies group management, not conversational reset. /clear deletes only caller-owned retained eligible selected-scope history and resets context. /export returns only eligible owner text privately, including explicit empty outcome. Group invocation never publishes a shared transcript. Opt-out is distinct from reset/clear.
- Global/group chat OFF prevents every AI route. Mention mode requires exact bot addressing/entity, reply to this bot, /ask or matching prompt; all mode permits ordinary eligible inputs. Admin-only adds fresh current group admin authority to **every** route. Commands addressed to another bot, bot messages and verified automatic discussion forwards do not trigger AI. Manual forwards do not convey authority and follow normal group eligibility.
- Platform-verified anonymous exact-group principals can perform permitted group-only administration/ephemeral AI, never become a named history/wallet/vote owner or global/private superadmin. A special numeric anonymous-user ID alone is not proof. If current authority cannot be proven, deny.
- Bound replies/callbacks keep original actor/tenant/topic/action/prompt ID. Expired/stale management replies never fall through into AI. New commands cancel stale ask prompts. AI edits do not auto-answer again; Ads text/caption edits use latest policy without recharging/repeating completed effects.
- Text/caption input only; no binary/OCR/transcription/URL fetch. All confirmed-answer routes share feedback: private owner only; group one immutable vote per current identifiable member/logical answer, identical replay idempotent and conflicting second vote rejected. Multi-chunk feedback appears on the final confirmed chunk only. Unknown reports are deduplicated owner/tenant metadata; no raw Q&A bypass.
- Explicitly **new** private-superadmin panel interaction: Direct-message policy → numeric user ID → Unblock explicit denial → bound confirmation. No invented existing /unblock command. Unblock clears denial only, no allowance or feature bypass.
- Locale: private owner's explicit choice or group explicit choice → managed global → deployment default → supported initiating-user hint → fa. Management text uses administrator private locale while naming target group. Normalize supported regional hints to fa/en/ar/tr/ru; unsupported saved values are rejected. Complete catalogs are a release gate; runtime corruption uses an already validated generic safe localized message and alert, never raw keys/provider errors.

### Verified names and intentional route repairs

| Family | Names |
| --- | --- |
| General | /start /help /whoami /health /ask /clear /export |
| Group management | /manage /panel /chat /lang /tokens /unknowns /fixcommands |
| Private global | /dm /allow /block /users /sa /audit /loglevel /lognoise, private /tokens and /ads_debug |
| Wallet aliases | /wallet /bal /balance |
| Ads standalone | /ads /ads_on /ads_off /ads_status /ads_action /ads_threshold /ads_add /ads_notad /ads_clear /ads_list /ads_probe /ads_wuser /ads_wdomain /ads_examples_clear /ads_examples_stats /ads_stats /ads_simulate /ads_debug |

Preserve dispatched /ads synonyms: status|info|?, on, off, action, threshold, list, chatflow, fewshots, balance|bal, gap, wuser, wdomain, stats, simulate; autoclean|autodel|autodelete; reply|reply_exempt; replylen|reply_maxlen; replycontact|reply_contact; replycontactlen|reply_contact_maxlen; level; mediatypes|media_types; captionlen; captionmetric; nocap_grace; allow_forward|forward; fwd_captionlen; fwd_grace; reply_as_caption|replyascaption; warn_success_action; warn_success_autodel|warn_success_autodel_sec; mute_hours|mutehours; examples count/stats/recent|list/clear YES. Repair intended /ads add and /ads notad to their standalone canonical operations, exactly once. /ads balance|bal controls balanced/latest examples, **not wallets**. /ads_probe is standalone; no compound probe route is invented.

Unsafe legacy cleanup/escalation values are not accepted merely because their command names remain: return a localized retired/unsupported-value explanation; bot-owned cleanup remains available. Provider selection/chatflow is a reviewed configured identifier, not authority for arbitrary URL/egress or raw provider prompt overrides.

The three selected-tenant example-clear aliases /ads_clear confirm, /ads_examples_clear YES, /ads examples clear YES require a prior live original-target/actor/action-bound confirmation. Parse actual command prefix once, preserving body text; foreign bot suffix, unknown command or malformed args cause no provider/mutation.

Pinned evidence: [bot registration](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L553-L605), [private admin registration](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/admin_commands.py#L904-L914), [wallet aliases](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/handlers.py#L148-L149), [Ads dispatch](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_commands.py#L279-L754), [Ads registration](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_commands.py#L2588-L2629). Registration is evidence of intent, not proof of working legacy runtime. Case details: AC-BR002–009/013/017; FR-007–023/045–047.

## D-011 — First-cutover entity dispositions

Every retain below is conditional on separately approved **actual sanitized mapping/evidence**. No real import is approved or executed.

| Entity | Selected intent |
| --- | --- |
| Users/tenants | Retain validated necessary IDs/ownership/membership links and supported locale; no unnecessary names/contact profile. |
| Wallet/ledger/grant/dedupe | Preserve validated balance/associated immutable records and minimum linked actual-outcome/period/safety evidence; never fabricate a balancing event or false successful spend. |
| Typed settings/global configuration | Stage only supported valid bounded values; no guessed permission translation, secret/provider prompt override or automatic-deletion activation. |
| Private allow/deny | Retain only demonstrably explicit applicable state, under new deny semantics; missing/deleted allow is not block. |
| Roles/superadmin | Rebuild current group authority; separately owner-reviewed new bootstrap identity list, never historic role flags or legacy secrets as current authority. |
| Panel/callback/force-reply/session/provider context/old storage consent | Exclude; fresh scope/context and default-off history consent. |
| Ads allowlists | Retain validated tenant-scoped normalized identities/provenance; historical global entries cannot become all-tenant exemptions. |
| Ads examples/content prompts | Exclude old raw examples/provider overrides; start curated examples empty. |
| History/feedback/unknown-answer content | Exclude all old Q&A/context links; metadata aggregates only if validated nonreconstructible and separately mapped. |
| Historical audit | Retain validated redacted non-content historical attribution, not current authority or invented outcome proof. |
| Pending effects | Retain only validated safety identity and known/unknown disposition as non-executable history/NO_REPLAY hold; no imported claim automatically executes. Ambiguous identity/outcome blocks affected promotion. |
| Secrets/host values/generated/corrupt/orphaned/ambiguous data | Exclude/reject/quarantine with exact reasons/counts, no secret values or inferred tenant/time/type. |

Each candidate source identity has exactly one imported-unchanged, transformed-imported, rejected, quarantined or excluded disposition. Independent counts/scopes/checksums/actual balance-event totals reconcile exactly. Unknown mapping, financial discrepancy or safety identity blocks affected promotion/cutover; quarantine expiry does not resolve it. Old source remains untouched. Trace: FR-048–054; AC-BR019-01–05; HIS-04; SC-007.

## D-012 — Bounded reference and corpus contract

Existing selected Standard/base+2× counting/samples/test windows in [measurement-contract.md](measurement-contract.md) remain. Additional values below are **recommended product/reference bounds**, not measured upstream/host capacities or architecture choices.

| Boundary | Selected default / maximum |
| --- | --- |
| Question/example | 4,000 Unicode scalars AND 16 KiB UTF-8 |
| Caption | 1,024 Unicode scalars AND 4 KiB UTF-8 |
| Structured update / callback | 256 KiB / 64 UTF-8 bytes |
| Album | 10 members; settle after 2 seconds quiet, at most 5 seconds from first member |
| Provider answer | 16,000 UTF-16 units; safe ordered chunks <=3,000 units, <=6 chunks including truncation notice |
| Consented context | Live own last 20 messages, <=16,000 scalars; remove oldest whole selected entries, not stored-history mutation |
| Pages / examples / fewshots | <=50 entries/page; <=500 examples/tenant; 0–50 selected, default 10 |
| Ask/admin prompt/confirmation | 5-minute absolute expiry |
| Active panel | 15-minute idle and 1-hour absolute expiry; authorize every use |
| Chat gap / bot cleanup | 0–3600 seconds default 2 / OFF or 30–3600 default OFF |
| Generic refusal guidance | At most one per chat/topic per 30 seconds, never notice spam in passive none/off AdsGuard |
| Per tenant+actor interactive | 30 admissions per rolling minute; at most 5 in any second |
| Per tenant+actor AI / per tenant AI | 6/minute, at most 2/second / 120/minute, at most 10/second |
| Per tenant+actor privileged writes | 10/minute, at most 3/second |
| Global logical / physical ingress | 100 new logical admissions/second / 200 physical deliveries/second |
| New active work | 10,000 global, 500/tenant; reject before admission, never evict accepted items |
| Admission age | <=24 hours from trusted original creation, active source epoch; edits do not refresh |
| Queue age | 120 seconds from admission to first dispatch for runnable work; progress indicators do not reset the clock. Scheduled grace waits become runnable at their original due time and start this clock then. Breach produces a safe terminal or durable review, never permission to retry unknown effects |
| Synthetic test resource budget | <=4 CPU equivalents / 8 GiB total RAM for declared evaluated product/dependencies; not server provisioning or topology |

Empty/oversize/malformed inputs are not sent to providers or truncated silently. Bound output is safely truncated at complete character/entity boundaries with a localized notice; no unsafe URL/markup execution. Delivery/partial chunks follow D-003. No binary media ingestion/download is selected. Settings outside bounds are rejected without changing prior effective state.

Reference manifest must deterministically assign every journey/category/locale/tenant/actor/topic/timestamp/expected route and remainder before results. Healthy Standard/2× traffic must fit selected per-scope limits and gaps; duplicates keep identity/no new slot, but physical delivery cost is counted. Negative/rate/late/oversize/fault cases are predeclared separately, not removed afterwards. Controlled reference dependencies: AI=2 seconds, classifier=0.5 seconds, transport confirmation=0.05 seconds for each externally visible operation; finite fake-transport ceiling 200 operations/second per declared test-bot identity and 10/second per synthetic chat. Reference AI answers are one chunk of <=1,000 UTF-16 units; worst-case six-chunk delivery and real upstream rate limits are separate boundary/integration tests. Use a 30-minute fixed journey schedule, deterministic round-robin actor/tenant/category/locale allocations and predeclared remainders; all ordinary mode, gap and quota restrictions remain in force. Base and 2× use the same finite resource/dependency bounds; declare any additional dependency resources rather than hiding them. These controlled rates are test doubles, not permitted Telegram rates. Real Telegram/provider limits and actual environment identities remain plan/integration gates.

Corpus coverage recommendation: held-out evaluation has at least 200 independently adjudicated items per fa/en/ar/tr/ru, at least 100 eligible AD and 100 legitimate non-exempt NON_AD per locale (>=1,000 total), plus declared exemption/UNKNOWN/structural variants. For each locale include neutral/contact-like/quoted-report/short/long text, caption/forward/edit/album and boundary adversaries; logical/member labels are separately reviewed. Training/seed items do not overlap held-out evaluation. Independent reviewers label blind to candidate outputs; adjudicate disagreement/REVIEW before freeze. The existing 30 AI-drafted seeds remain unapproved, not magically transformed into gold. Owner approves actual corpus/coverage revision separately before D-005 evaluation/activation; population recommendation is not approval of unseen labels.

Primary platform constraints were checked against [Telegram Bot API](https://core.telegram.org/bots/api#sendmessage), [message entities](https://core.telegram.org/bots/api#messageentity), [getting updates](https://core.telegram.org/bots/api#getting-updates) and [Bots FAQ](https://core.telegram.org/bots/faq#how-do-i-avoid-hitting-limits). Product bounds may be more restrictive. Telegram's 24-hour update storage is not proof of bot-side deduplication, and selected reference throughput is not a guarantee of live upstream capacity.

## D-013 — Collection, retention, backup and provider processing

Durations are selected working product targets, **not legal-compliance claims or verified current deployment/provider facts**. All expiry clocks use original creation/event UTC unless the separate imported-detail clock is explicitly stated.

| Category | Lifecycle recommendation |
| --- | --- |
| New Q&A/history/context | Existing default-off, own opt-in after notice, original creation+30×24h UTC, earlier clear/export; no consent backfill or expiry reset. |
| Necessary live profile/tenant/wallet/settings | Minimal while enrolled; verified explicit closure removes operational state within 7 days, distinct required evidence follows its term. Clearing history is not wallet/account closure. |
| Feedback/unknown-answer/Ads ordinary metadata | 30 days, current owner/tenant scope, no raw Q&A/context references. |
| Logs/traces | Redacted non-content, 14 days. |
| Nonreconstructible anonymous aggregates | 90 days, no user/message IDs. |
| Privileged audit | Redacted tamper-evident, 365 days from event, currently scoped authorized access. |
| Ledger detail | 365 days from event; validated imported detail at least 365 days after later approved cutover, preserving original event time. Exact independently reconciled non-event checkpoint before detail expiry preserves balance/provenance/count/hash/current-period/safety proof; never fabricated grants/debits. |
| Compact grant/spend/external-effect outcomes | 365 days; purge only after proved age/epoch barriers prevent old replay and known identity/outcome cannot be reopened. |
| Unknown external review | Active <=30 days, then archived unresolved/NO_REPLAY; compact evidence365 days, never timeout-as-failure/success/refund. |
| Fresh Ads examples | Synthetic/nonpersonal approved curated versions, active version review every365 days; remove within90 days of retirement; explicitly permitted submitter-authored personal text original creation+30 days. No copying members' private/history text. |
| Future authorized sanitized import staging | <=7 days, encrypted/restricted; remove content/context/secrets before persistence; fresh authorization/snapshot if later needed. |
| Quarantine payload / reconciliation report | Minimal sanitized payload30 days / redacted report365 days; expiry never removes unresolved disposition or permits promotion. |
| Backups | Project-isolated encrypted metadata-only, <=30 days; exclude all Q&A/context/raw examples/provider payloads. Disclose offline expired metadata may remain up to30 extra days; restore purges expired/closed state before exposure and preserves clocks/safety barriers. |

Metadata-only backup intentionally does **not** preserve conversation text: lost active history cannot be restored from it and users must be told before opting in. Recovery objective recommendation: zero unexplained loss/duplication of acknowledged wallet/grant/debit/audit/safety outcomes; if evidence cannot demonstrate this, affected state stays unready and recovery is not signed off. Other ordinary operational metadata may have <=24-hour recovery-point age; conversation text has no backup-restoration guarantee. A plan must prove the chosen recovery/data-loss contract; no drill or guarantee of an uninspected system is asserted.

Provider requirement: only necessary current eligible input and own eligible opted-in context, localized processing notice distinct from optional history consent. Group processing policy must be disclosed; group enablement is not member history consent. Select no persistent prompts/responses/classification text, no training/reuse/raw diagnostic logging, request-bound transient processing only. No silent cross-provider/region fallback or stored session reference bypass. Actual retention/subprocessors/regions/residency/deletion/training terms remain UNKNOWN; integration disabled until reviewed agreement and technical evidence match this contract. If unavailable, propose explicit amendment, never falsely label compliant. This does not promise deletion of Telegram's original messages.

Access/export/erase: own eligible user data only through currently verified identity/scope, other-actor audit content redacted; administrators cannot export another owner's private text. Ledger/audit corrections are append-only/attributed, not erased by history clear. Verified closure and expiry minimize ordinary profile metadata without destroying still-required exact ledger/safety proof; no blanket indefinite personal-data retention.

Trace: FR-034/048–059/064; AC-BR004-02/03, AC-BR005-04, AC-BR013-02/05, AC-BR018-01/02, AC-BR019-04/05; HIS-01–04. Consent/expiry/restore adversaries still test deliberately old unsafe backup fixtures even though newly selected backups exclude history.

## Approval and evidence gates

- POLICY_REVIEW: owner explicitly approves this consolidated specification/recommendation revision and preserve/change/retire inventory. Recommendation delegation is not this whole-spec approval.
- PLAN_GATE: only after policy approval, separately authorize planning; actual sanitized schema/key/count/current-role/import maps, processor/subprocessor/region agreements and recovery-capacity evidence are required before dependent migration/architecture plan approval. No current server access is authorized.
- IMPLEMENTATION_GATE: separate approved architecture/migration plan, requested test-first work; no implementation now.
- RELEASE_GATES: independent owner-approved gold corpus and actual passing evaluation; required audit/ledger/replay/checkpoint/restore/locale/Docker/security verification, rotated secrets and measured integration envelope.
- DELETION_ACTIVATION: separate explicit owner activation for tested scope/config; absent now. A safe structural reminder or selected delete setting never grants it.
- SERVER_GATE: any future server sync/migration/deploy requires new explicit authorization and consumes a reviewed GitHub revision; ai_bot6 remains immutable.

All delayed/privileged actions must re-check original actor/target authority, current permissions, policy and consent immediately before dispatch. Temporary network failure gives UNKNOWN authority, never proof of exemption absence or a guessed role. Fresh conversation input is required for new AI effects; closed/blocked tenants cannot execute recovered work. The exact deployment identities and approved external processing agreements belong to the plan, not to the synthetic fixture.

No evidence gate is closed by invented data, static registration, an AI label or a timeout. These are reviewable contracts, not passing results.

## Cross-contract consistency boundaries

- Every continuation/callback binds actor, tenant, topic, operation, request/session generation and exact prompt or answer ID. Reset/clear invalidates old generations; late results cannot repopulate erased history or new context. Stale prompts/invalid or foreign commands never fall through to AI. Prompt expiry governs new submission/confirmation, not the safe disposition of admitted work.
- Borrowed input requires legitimate same-scope visibility. The entire exchange, including derived answer/summary, stays transient unless EVERY contributing owner has valid in-scope consent and eligible unexpired/uncleared source text. Requester consent alone cannot transfer ownership. Group exports are private-only; unavailable/unknown private delivery never produces a public transcript or false success. DM-conversation denial does not remove independent self-service/management authority.
- Unknown reports are logical-answer-owner only; callbacks verify original tenant/topic/answer. Anonymous authority requires current exact-group platform evidence, not a reserved ID or forward origin; quota principals remain tenant-bound. Output bounds measure final safe rendered text including truncation notice; interpolation/escaping cannot bypass them.
- UNKNOWN implies no NEW/uncommitted debit, never resetting/refunding a proven committed one. Preserve external confirmation and debit outcomes separately. Origin epochs/fences are trusted provenance, not fresh tags assigned to old input or rotated on restart/settings. Restore/cutover preserves/reconciles the original input fence and critical outcome checkpoint; pre-fence IDs are rejected even at <=24h age. A forward's age uses the original creation of its NEW Telegram message, not the older forwarded source; edits never refresh that date.
- Only current UTC grant week is eligible; fresh jobs cannot backfill closed periods after outcome-detail purge. Scheduled eligibility checks and explicit balance queries are selected grant triggers. AI/AdsGuard/debit processing does NOT implicitly grant. Imported validated same-week grant/cap-skip proof prevents a duplicate. Linked proof/checkpoints and current-period fences survive detail purge.
- Caption correction removes stale structural grounds only, not independently valid AD evidence. Lack of caption is never such evidence. Exact album member dispositions/IDs are required; late members/replies/protected content never append to an old deletion set merely through an album ID.
- Required bounded redacted telemetry production/correlation remains mandatory during optional-exporter outage; affected admission stops if required emission/evidence is unavailable. Alert production/delivery/resolution remain distinct; all-sink loss cannot pass audit/alert coverage.
- RPO0 is the selected target for acknowledged wallet/audit/replay/security/privacy/deletion-safety invariants within the plan's approved failure envelope, NOT a daily-backup guarantee. This includes current denials/revocations/policy/activation scope and consent/clearing. Lost proof requires fail-closed revalidation; lost consent defaults OFF. Ordinary <=24h metadata RPO excludes those critical states. The plan must prove exact critical recovery from a suitable journal/checkpoint before release; no invented events or restored stale authority.
- Future separately authorized identified import staging is NOT a CI fixture. Test/corpus material is synthetic or irreversibly anonymized; no production IDs/credentials/backups or raw mappings are published in GitHub/tests. Actual mapping/count evidence is reviewed through appropriately sanitized records.

Unavailable status is not an unconditional Telegram fallback send: dispatch a localized notice only if that notice itself has safe correlation/outcome tracking. Otherwise do not dispatch a new external effect and expose readiness through an available safe channel. Required Flowise initial adapter/product support remains a constitutional constraint; no unverified endpoint is enabled. Verified closure never reopens a grant period, refunds a balance or deletes another tenant's data.
