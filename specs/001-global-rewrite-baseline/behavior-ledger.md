# Legacy behavior traceability ledger

Status: Consolidated selected working recommendations, reviewed 2026-10-11; whole-scope owner approval pending. Decision owner for every record: project owner. Source: immutable ai_bot6 commit `cdb0efd4b39eaafd1265822350606887fe3b40b3` (historical snapshot, not a claim about today's server). Specification: [spec.md](spec.md); decisions: [decision-register.md](decision-register.md); verification definitions: [acceptance-cases.md](acceptance-cases.md).

## Reading this ledger

Evidence is static pinned-source verification, not a runtime test or exhaustive discovery claim. Registration and intended helper calls do not prove a working journey. Historical Observed/Evidence paragraphs are unchanged. Preserve means the capability, not unsafe legacy branches. Explicit historical decisions stay within their original limits; remaining working recommendations are selected under owner delegation, not whole-spec approval.

[recommended-contracts.md](recommended-contracts.md) defines exact policy choices. [review-summary.md](review-summary.md) exposes their tradeoffs and remaining evidence/approval gates. All 23 records below now carry a selected working classification/impact/communication path; actual schema/authority/provider facts, independent gold corpus, whole-spec/plan approval, test results, activation and server permission remain separate. Cases are definitions only, NOT RUN.

## BR-001 — Discovery, identity, menus and health

Actors: user, group administrator, superadmin. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-009/010/012/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Group start gives a short ready message; private start rotates the chat reference and opens home. Menus differ by scope. whoami reports the caller's identity. Health queries dependencies and may expose exception/provider details.

Evidence: [telegram_bot/bot.py:323–409](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L323-L409); [telegram_bot/bot.py:553–575](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L553-L575); [telegram_bot/user_commands.py:284–293](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L284-L293); [telegram_bot/user_commands.py:352–409](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L352-L409); [telegram_bot/user_commands.py:1136–1159](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1136-L1159); [telegram_bot/user_commands.py:2095–2166](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L2095-L2166).

Classification: Preserve discovery/menus; Change redacted health and current scope (selected working recommendation for owner review).

User/data/security impact: Applicable menus are not authority. Health is redacted; mandatory evidence/outcome outage blocks affected operations without unsafe fallback notices. Exact ranges/transitions: recommended-contracts.md (D-009/010/012/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-009/010/012/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR001-01, AC-BR001-02, AC-BR001-03, AC-BR001-04; US2.1; US7.1/4; US8.1; FR-007, FR-013, FR-022, FR-045, FR-046, FR-047, FR-055, FR-056, FR-061. Cases are definitions only, not executed/passed. Apply the precise D-009/010/012/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-002 — Private questions, ask and chunked replies

Actors: authorized private user and group member. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-010/012/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Plain private questions use a stable provider session without the same local history/feedback path as ask. ask accepts arguments or replied text/captions and otherwise opens a prompt. Shared provider gating still enforces global chat OFF. Long answers are chunked.

Evidence: [telegram_bot/bot.py:586–605](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L586-L605); [telegram_bot/user_commands.py:1233–1297](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1233-L1297); [telegram_bot/user_commands.py:1375–1440](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1375-L1440); [telegram_bot/user_commands.py:1555–1637](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1555-L1637); [telegram_bot/user_commands.py:1699–1736](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1699-L1736); [telegram_bot/shared_utils.py:448–511](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L448-L511); [telegram_bot/shared_utils.py:1974–2041](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1974-L2041).

Classification: Preserve AI questions; Change uniform route/context/feedback contracts (selected working recommendation for owner review).

User/data/security impact: Plain-private, /ask, visible reply text/caption and bound prompt share gates and feedback. No history means current exchange only; borrowed-owner exchange including answer stays transient unless every owner has eligible consent. Rendered output obeys chunk bounds and per-part D-003 unknown holds. Exact ranges/transitions: recommended-contracts.md (D-010/012/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-010/012/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR002-01, AC-BR002-02, AC-BR002-03, AC-BR002-04; US2.1/5/7–10; FR-008, FR-009, FR-011, FR-012, FR-013, FR-014, FR-055, FR-056, FR-058, FR-063, FR-064. Cases are definitions only, not executed/passed. Apply the precise D-010/012/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-003 — Group conversation and cleanup

Actors: member, current/anonymous group administrator. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-007/010/012 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Disabled/mention/all, administrator-only and per-chat/topic gaps exist. Auto-forward handling and addressed ordinary-user behavior in administrator-only mode differ from a simple role restriction. Some cleanup deletes the original user's trigger, not just bot messages.

Evidence: [telegram_bot/user_commands.py:97–225](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L97-L225); [telegram_bot/user_commands.py:1299–1373](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1299-L1373); [telegram_bot/user_commands.py:1442–1553](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1442-L1553); [telegram_bot/user_commands.py:1738–1852](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1738-L1852); [telegram_bot/shared_utils.py:1905–1928](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1905-L1928); [telegram_bot/shared_utils.py:2045–2082](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L2045-L2082); [telegram_bot/panel_ui.py:855–889](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/panel_ui.py#L855-L889).

Classification: Preserve group off/address/all/admin-only; Change uniform gates; Retire hidden user cleanup (selected working recommendation for owner review).

User/data/security impact: Every route including /ask rechecks current group policy/admin-only role/topic. Verified anonymous group principal is scoped ephemeral, not wallet/history/vote/global. Original-user AI/command auto-cleanup is retired; bot-owned housekeeping only. No hidden deletion or unsolicited none/off notice. Exact ranges/transitions: recommended-contracts.md (D-007/010/012). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-007/010/012 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR003-01, AC-BR003-02, AC-BR003-03, AC-BR003-04; US2.2–5; US3.2; FR-010, FR-011, FR-013, FR-014, FR-016, FR-017, FR-020, FR-028, FR-043, FR-044, FR-055, FR-056, FR-058, FR-064. Cases are definitions only, not executed/passed. Apply the precise D-007/010/012 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-004 — Sessions, history, clear and export

Actors: history owner; group administrator cannot opt in for others. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-010/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Legacy clear rotates a reference rather than deleting prior rows. Group export can expose a shared current session. ask and plain-private context identifiers differ. Local history append has no explicit consent/30-day expiry.

Evidence: [telegram_bot/user_commands.py:1163–1229](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1163-L1229); [telegram_bot/shared_utils.py:1648–1714](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1648-L1714); [telegram_bot/shared_utils.py:1725–1783](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1725-L1783); [telegram_bot/user_commands.py:1612–1637](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1612-L1637); [telegram_bot/user_commands.py:1712–1736](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1712-L1736).

Classification: Preserve history/clear/export; approved D-001/D-004; Change uniform reset and borrowed-text ownership (selected working recommendation for owner review).

User/data/security impact: First cutover history/context empty. New storage off until own opt-in; original+30d expiry/earlier clear. /start resets context/home not history/consent; /clear removes own history and resets generation; late results cannot restore it. Export private-only; metadata backups never contain history so disaster loss is not restored. Exact ranges/transitions: recommended-contracts.md (D-010/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-010/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR004-01, AC-BR004-02, AC-BR004-03, AC-BR004-04; US2.6/8–10; US5.1/6; FR-011, FR-012, FR-048, FR-049, FR-055, FR-058, FR-059, FR-064. Cases are definitions only, not executed/passed. Apply the precise D-010/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-005 — Feedback, unknown reporting and review

Actors: answer owner/voter; scoped administrator. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-009/010/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Private/group feedback differs, with per-user deduplication. Raw unknown questions are captured. Report-by-ID and unknown listing lack complete owner/tenant constraints. One unknown-reply helper call disagrees with the helper signature; intended behavior is not verified successful behavior.

Evidence: [telegram_bot/bot.py:578–580](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L578-L580); [telegram_bot/user_commands.py:229–265](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L229-L265); [telegram_bot/user_commands.py:1896–1976](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1896-L1976); [telegram_bot/shared_utils.py:1786–1886](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1786-L1886); [telegram_bot/admin_commands.py:469–510](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/admin_commands.py#L469-L510); [telegram_bot/shared_utils.py:2045–2082](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L2045-L2082).

Classification: Preserve feedback/report utility; Change confirmed-answer ownership/scope/finite lifecycle (selected working recommendation for owner review).

User/data/security impact: Private feedback/report owner-only; named current group members one immutable vote, duplicate idempotent/conflicting second rejected. Controls bind confirmed logical answer/topic/scope. Unknown review exposes only currently authorized metadata and never retains Q&A without eligible owner consent; metadata30d. Exact ranges/transitions: recommended-contracts.md (D-009/010/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-009/010/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR005-01, AC-BR005-02, AC-BR005-03, AC-BR005-04; US2.7/10; US3.3; FR-015, FR-016, FR-017, FR-023, FR-055, FR-056, FR-058, FR-059. Cases are definitions only, not executed/passed. Apply the precise D-009/010/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-006 — Direct-message access and user listing

Actors: private user, superadmin; group administrator has no implied global power. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-006/009/010/011 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: dm toggles require private superadmin, but allow/block paths can let a group admin change global allowed_dm. block deletes an allow row, not an overriding denial: global-on or environment allow can still permit access. User-list query/render fields disagree.

Evidence: [telegram_bot/admin_commands.py:61–86](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/admin_commands.py#L61-L86); [telegram_bot/admin_commands.py:129–175](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/admin_commands.py#L129-L175); [telegram_bot/admin_commands.py:289–466](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/admin_commands.py#L289-L466); [telegram_bot/shared_utils.py:1533–1558](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1533-L1558).

Classification: Approved Change D-006 explicit denial; Change safe global mutation/list/unblock interaction (selected working recommendation for owner review).

User/data/security impact: Explicit DM denial wins until current superadmin explicit unblock, no allowance/restart bypass. NEW private DM-policy panel numeric target plus bound confirmation clears denial only. Global writes/reads scoped to current SA; own self-service remains independently authorized. Missing legacy allow row is not denial; actual imports conditional. Exact ranges/transitions: recommended-contracts.md (D-006/009/010/011). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-006/009/010/011 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR006-01, AC-BR006-02, AC-BR006-03, AC-BR006-04; US2.4/11; US3.3/4; FR-008, FR-016, FR-017, FR-021, FR-022, FR-023, FR-043, FR-044, FR-055, FR-056, FR-059. Cases are definitions only, not executed/passed. Apply the precise D-006/009/010/011 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-007 — Private management and bound prompts

Actors: current scoped group administrator. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-009/010/012 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Deep links bind a group after verification; an active-group picker is stored per user. Some reads lack fresh role checks. Force-reply editing stores field/module but not a fixed group or expected prompt, then writes to the currently active group. Comments about deleting old panels do not prove implementation.

Evidence: [telegram_bot/user_commands.py:295–350](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L295-L350); [telegram_bot/user_commands.py:412–448](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L412-L448); [telegram_bot/user_commands.py:563–608](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L563-L608); [telegram_bot/user_commands.py:656–849](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L656-L849); [telegram_bot/user_commands.py:1065–1132](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1065-L1132); [telegram_bot/panel_ui.py:524–620](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/panel_ui.py#L524-L620); [telegram_bot/shared_utils.py:1228–1285](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1228-L1285).

Classification: Preserve management capability; Change current actor/target/topic/generation binding (selected working recommendation for owner review).

User/data/security impact: Bound five-minute prompt/confirm, panel15min idle/1h absolute. Current scope rechecked every use; changing active group cannot retarget; stale/foreign/malformed reply cannot fall through to AI. Expiry governs new submission, not already admitted safe disposition. Exact ranges/transitions: recommended-contracts.md (D-009/010/012). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-009/010/012 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR007-01, AC-BR007-02, AC-BR007-03, AC-BR007-04; US3.1/2/4; FR-017, FR-018, FR-019, FR-020, FR-023, FR-043, FR-044, FR-055, FR-056. Cases are definitions only, not executed/passed. Apply the precise D-009/010/012 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-008 — Roles, superadmins and privileged audit

Actors: regular/admin/anonymous-admin/superadmin/operator. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-009/010/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Current membership checks generally fail closed. Anonymous group authority is sender-chat scoped, not private superadmin authority. Superadmins are database-first with environment bootstrap. Separate legacy user-admin flags exist. Audit writes can swallow failures.

Evidence: [telegram_bot/admin_commands.py:42–125](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/admin_commands.py#L42-L125); [telegram_bot/admin_commands.py:963–1098](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/admin_commands.py#L963-L1098); [telegram_bot/shared_utils.py:1289–1348](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1289-L1348); [telegram_bot/shared_utils.py:1420–1439](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1420-L1439); [telegram_bot/shared_utils.py:1479–1522](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1479-L1522); [telegram_bot/shared_utils.py:706–751](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L706-L751).

Classification: Preserve scoped roles/audit; Change verified anonymous authority and fail-closed mandatory evidence (selected working recommendation for owner review).

User/data/security impact: Current exact-scope roles and actor-target evidence required. Mandatory redacted audit BEFORE privileged processing; unavailable sink blocks affected admission. No false recorded denial/success; network arrival is not processed admission. Every processed accepted/denied attempt audited; optional exporter outage cannot waive required emission. Exact ranges/transitions: recommended-contracts.md (D-009/010/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-009/010/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR008-01, AC-BR008-02, AC-BR008-03, AC-BR008-04; US3.3/4; US8.2; FR-016, FR-017, FR-021, FR-023, FR-055, FR-056, FR-057, FR-059, FR-063. Cases are definitions only, not executed/passed. Apply the precise D-009/010/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-009 — Scoped settings, language and menus

Actors: group administrator; superadmin for global settings. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-007/009/010/012 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Chat/Ads controls expose bounds, but those observed numbers are not approved new defaults. Language selectors expose five choices. Some persistence failures are swallowed before success output; some private-language callbacks lack equivalent private-scope checks.

Evidence: [telegram_bot/panel_ui.py:247–278](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/panel_ui.py#L247-L278); [telegram_bot/panel_ui.py:355–504](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/panel_ui.py#L355-L504); [telegram_bot/panel_ui.py:746–889](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/panel_ui.py#L746-L889); [telegram_bot/admin_commands.py:180–285](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/admin_commands.py#L180-L285); [telegram_bot/admin_commands.py:523–609](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/admin_commands.py#L523-L609); [telegram_bot/shared_utils.py:995–1007](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L995-L1007); [telegram_bot/user_commands.py:978–1060](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L978-L1060).

Classification: Preserve scoped settings/menus; Change fresh scope and truthful confirmation (selected working recommendation for owner review).

User/data/security impact: Global writes current superadmin only; target/revision-bound confirmations, reject stale concurrent revision without false saved notice. Exact locale/preset previews apply; choosing delete/preset is never owner activation. Restore cannot revive revoked policy/consent/permission. Exact ranges/transitions: recommended-contracts.md (D-007/009/010/012). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-007/009/010/012 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR009-01, AC-BR009-02, AC-BR009-03, AC-BR009-04; US3.2/4; US7.2/3; FR-017, FR-020, FR-021, FR-022, FR-023, FR-024, FR-027, FR-028, FR-042, FR-043, FR-044, FR-045, FR-046, FR-047, FR-055, FR-056, FR-063, FR-064. Cases are definitions only, not executed/passed. Apply the precise D-007/009/010/012 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-010 — AdsGuard classification and eligibility

Actors: group member; administrator configuring exemptions. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-007/012/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Registration reaches fresh photo/video and text/reply paths; all edits are routed, but the handler exits early for non-media edits. Admin/user/domain/reply exemptions and bot-address shortcuts exist. Missing/unparsable scores can be treated as AD. Decision rows may contain raw text.

Evidence: [telegram_bot/ads_commands.py:128–175](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_commands.py#L128-L175); [telegram_bot/ads_guard.py:2114–2124](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2114-L2124); [telegram_bot/ads_guard.py:2235–2258](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2235-L2258); [telegram_bot/ads_guard.py:2322–2416](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2322-L2416); [telegram_bot/ads_guard.py:2477–2505](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2477-L2505); [telegram_bot/ads_guard.py:2610–2747](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2610-L2747); [telegram_bot/ads_guard.py:1087–1115](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L1087-L1115).

Classification: Preserve classifier capability; Change strict scores, eligible text/captions and exemptions (selected working recommendation for owner review).

User/data/security impact: Structured AD/NOT_AD finite score>=threshold0.90; malformed becomes UNKNOWN. No binary/OCR/URL fetch. Known same-scope identity/admin/user/domain/short-reply exemptions before action; unknown authority blocks destruction/debit. Structural violation never replaces positive AD evidence. Exact supported origins and domain all-host rules are selected. Exact ranges/transitions: recommended-contracts.md (D-007/012/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-007/012/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR010-01, AC-BR010-02, AC-BR010-03, AC-BR010-04; US4.1–6/10; FR-025, FR-026, FR-027, FR-030, FR-054, FR-055, FR-056, FR-058. Cases are definitions only, not executed/passed. Apply the precise D-007/012/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-011 — Modes, forward/caption guards, profiles and mute

Actors: group member/admin; owner for deletion activation. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-007/008/012 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Disallowed entity-forwards can delete before classifier/action lookup. Captionless-media grace can schedule deletion before mode lookup. Profiles combine caption/forward switches with none/delete. Non-delete paths spend tokens; insufficient balance forces deletion. Manual mute callbacks may also delete associated user content.

Evidence: [telegram_bot/ads_commands.py:325–541](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_commands.py#L325-L541); [telegram_bot/ads/profiles.py:44–123](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads/profiles.py#L44-L123); [telegram_bot/ads_guard.py:2445–2473](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2445-L2473); [telegram_bot/ads_guard.py:2509–2608](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2509-L2608); [telegram_bot/ads_guard.py:1143–1169](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L1143-L1169); [telegram_bot/ads_guard.py:2783–2840](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2783-L2840); [telegram_bot/ads_guard.py:2981–2990](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2981-L2990); [telegram_bot/ads_guard.py:3064–3167](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L3064-L3167); [telegram_bot/ads_guard.py:1321–1448](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L1321-L1448).

Classification: Approved D-002/D-005/D-007; Change structural-only deletion/presets; Retire hidden mute/user cleanup (selected working recommendation for owner review).

User/data/security impact: Current off/none no moderation; warn cannot delete; delete requires positive qualifying AD plus every current authorization/exemption/quality/activation safeguard. Caption300s/forward120s original grace, correction boundary selected; structural-only content never deletes. Mute24h confirmed NO associated deletion. Presets free:none, medium/strict:warn. User-content cleanup/escalation retired, bounded positively bot-owned cleanup only. No activation by publication. Exact ranges/transitions: recommended-contracts.md (D-007/008/012). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-007/008/012 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR011-01, AC-BR011-02, AC-BR011-03, AC-BR011-04, AC-BR011-05; US4.1/3/7–10; US6.4; FR-024, FR-025, FR-027, FR-028, FR-030, FR-053, FR-064. Cases are definitions only, not executed/passed. Apply the precise D-007/008/012 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-012 — Pending moderation and album recovery

Actors: operator and current scoped policy owner. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-007/009/012/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Pending upserts can reset status/attempts/claims. Claims and generic/stale retries exist. Album actions can have partial results. Independent cache TTL pruning can remove needed member IDs. Generic retry does not prove prior non-execution.

Evidence: [telegram_bot/ads_pending_store.py:60–209](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_pending_store.py#L60-L209); [telegram_bot/ads_pending_store.py:313–467](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_pending_store.py#L313-L467); [telegram_bot/ads_pending_jobs.py:19–193](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_pending_jobs.py#L19-L193); [telegram_bot/ads_album_jobs.py:14–58](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_album_jobs.py#L14-L58); [telegram_bot/bot.py:545–551](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L545-L551).

Classification: Preserve durable work; approved D-003; Change fenced/member-specific recovery (selected working recommendation for owner review).

User/data/security impact: Confirmed completion never repeats; proven nonexecution only bounded current-authorized retry; unknown review NO_REPLAY. Exact album members/outcomes preserved; no late member added to old deletes. Runnable queue120s and original grace due clock; compact365d proof expiry only demonstrated input-origin/current-period barriers. Restore missing critical fence/evidence fails closed. Exact ranges/transitions: recommended-contracts.md (D-007/009/012/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-007/009/012/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR012-01, AC-BR012-02, AC-BR012-03, AC-BR012-04, AC-BR012-05; US4.3/4/10; US8.2/4; FR-029, FR-030, FR-062, FR-063, FR-064. Cases are definitions only, not executed/passed. Apply the precise D-007/009/012/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-013 — Examples, allowlists and diagnostics

Actors: scoped group administrator; private superadmin for debug. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-009/010/011/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Compound stats/simulate uses a guard but standalone registered tools lack the same body guard. Some advertised compound add/notad aliases are not dispatched. Probe/simulation can send a diagnostic response and update ancillary records: legacy 'side-effect free' is not literally no writes/network.

Evidence: [telegram_bot/ads_commands.py:216–246](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_commands.py#L216-L246); [telegram_bot/ads_commands.py:279–308](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_commands.py#L279-L308); [telegram_bot/ads_commands.py:1652–1812](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_commands.py#L1652-L1812); [telegram_bot/ads_commands.py:1926–1969](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_commands.py#L1926-L1969); [telegram_bot/ads_commands.py:2061–2377](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_commands.py#L2061-L2377); [telegram_bot/ads_commands.py:2379–2629](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_commands.py#L2379-L2629); [telegram_bot/ads_guard.py:1767–1787](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L1767-L1787); [telegram_bot/ads_guard.py:1835–1954](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L1835-L1954).

Classification: Preserve scoped diagnostics/examples/allowlists; Change exact aliases/provenance/lifecycles (selected working recommendation for owner review).

User/data/security impact: Diagnostics cause no real moderation/debit. /ads add/notad deliberate repair, /ads balance|bal refers to example selection, standalone probe remains standalone. Destructive example clear bound-confirmed tenant only; safe retired user-cleanup values never reactivate deletion. Examples startempty atcutover, synthetic or own explicitly permitted provenance; no private/history copying. Exact ranges/transitions: recommended-contracts.md (D-009/010/011/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-009/010/011/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR013-01, AC-BR013-02, AC-BR013-03, AC-BR013-04, AC-BR013-05; US3.2; US4.2/5; FR-017, FR-018, FR-019, FR-020, FR-021, FR-023, FR-031, FR-032, FR-033, FR-034, FR-054, FR-055, FR-056, FR-058, FR-059. Cases are definitions only, not executed/passed. Apply the precise D-009/010/011/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-014 — Wallet balance, weekly grants and carry

Actors: eligible tenant user; scoped administrator. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-008/011/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Balance aliases select group or private active-group context and may trigger lazy grant. Grant logic awards one token within carry cap using week identity. Comments describe Monday scheduling but registration runs daily; weekly deduplication—not the schedule comment—controls duplicate grants.

Evidence: [telegram_bot/tokens/handlers.py:19–68](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/handlers.py#L19-L68); [telegram_bot/tokens/handlers.py:148–149](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/handlers.py#L148-L149); [telegram_bot/tokens/models.py:10–114](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/models.py#L10-L114); [telegram_bot/tokens/core.py:7–22](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/core.py#L7-L22); [telegram_bot/tokens/jobs.py:12–33](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/jobs.py#L12-L33); [telegram_bot/ads_commands.py:48–119](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_commands.py#L48-L119); [telegram_bot/bot.py:545–549](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L545-L549).

Classification: Preserve wallets; select current-week +1/carry 1 and explicit grant triggers (selected working recommendation for owner review).

User/data/security impact: Both token switches default OFF; +1 current MondayUTC week to verified non-bot member, carry 1 (0–100). Scheduled checks/explicit balance only triggers; no implicit Ads/AI/debit grant. First grant/SKIPPED_CAP closesweek, no missed-weekbackfill or cap-reduction burn. Imported valid same-week proof blocks doublegrant. Exact ranges/transitions: recommended-contracts.md (D-008/011/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-008/011/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR014-01, AC-BR014-02, AC-BR014-03, AC-BR014-04; US6.1/2/5; FR-035, FR-036, FR-037, FR-038, FR-040, FR-041, FR-044, FR-062. Cases are definitions only, not executed/passed. Apply the precise D-008/011/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-015 — Token spending and ledger outcomes

Actors: tenant wallet owner; reconciliation operator. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-008/011/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Dedupe identity is inserted before checking balance. An insufficient-balance attempt returns false but its later duplicate can return true without a debit. Dedupe cleanup defaults to seven days, allowing old replay without durable outcome proof. Delete-mode bypass and spending in none with sufficient balance are observed, not approved by D-002.

Evidence: [telegram_bot/tokens/models.py:245–333](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/models.py#L245-L333); [telegram_bot/ads_guard.py:2789–2815](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2789-L2815); [telegram_bot/tokens/jobs.py:37–58](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/jobs.py#L37-L58).

Classification: Preserve ledger invariants; approved D-008; Change confirmed-warning-only charging and finite fenced proof (selected working recommendation for owner review).

User/data/security impact: Only confirmed eligible warning costs 1 current warn/both switches ON/atomic balance>=1. none/off/delete/unknown/guard-only/AI cost0, exhaustedwarn still warns. No NEW unknown debit, never refund proven committed one. Identity unchanged edits/albums/settings; recorded no-charge/insufficient never retrocharges. Age<=24h/current origin/current grant week fences survive365d detailpurge; explicit proved linked correction only. Exact ranges/transitions: recommended-contracts.md (D-008/011/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-008/011/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR015-01, AC-BR015-02, AC-BR015-03, AC-BR015-04, AC-BR015-05; US6.3–6; US4.7–9; FR-039, FR-040, FR-062, FR-064. Cases are definitions only, not executed/passed. Apply the precise D-008/011/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-016 — Configuration precedence and startup seeding

Actors: group administrator, superadmin, operator. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-009/010/011/012 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Managed readers prefer database values then environment/default and fall back on invalid parse. Startup seeds missing global values only. Chat settings are separately scoped; this is not proof of one consistent legacy precedence everywhere.

Evidence: [telegram_bot/shared_utils.py:754–821](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L754-L821); [telegram_bot/shared_utils.py:859–910](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L859-L910); [telegram_bot/shared_utils.py:973–1012](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L973-L1012); [telegram_bot/bot.py:186–197](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L186-L197); [telegram_bot/bot.py:250–275](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L250-L275).

Classification: Preserve typed precedence; Change truthful save and no authority/config guessing (selected working recommendation for owner review).

User/data/security impact: Approved managed/global/deployment precedence and selected defaults are explicit. Startup seed cannot overwrite valid settings or infer privileges; current revision and target validation reject bad/stale update without false confirmation. No host-global tooling or arbitrary chatflow URL/prompt override. Exact ranges/transitions: recommended-contracts.md (D-009/010/011/012). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-009/010/011/012 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR016-01, AC-BR016-02, AC-BR016-03; US7.2/3; US3.2/4; FR-020, FR-021, FR-022, FR-023, FR-042, FR-043, FR-044, FR-056. Cases are definitions only, not executed/passed. Apply the precise D-009/010/011/012 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-017 — Localization coverage and fallback

Actors: user, group administrator, release reviewer. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-010/012 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Five locale selectors and chat→global→user-hint→Persian preference exist. Missing selected messages fall back to Persian then raw key; formatting failure returns base text. Persian has 170 top-level keys, English 21; en/ar/tr/ru share a blob. Selectable names are not complete translations.

Evidence: [telegram_bot/admin_commands.py:555–609](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/admin_commands.py#L555-L609); [telegram_bot/messages_service.py:24–156](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/messages_service.py#L24-L156).

Classification: Preserve five launch locales; Change exact fallback and full catalog gate (selected working recommendation for owner review).

User/data/security impact: Explicit private user/group→managed global→deployment→supported initiating user hint→Persian. Complete fa/en/ar/tr/ru catalogs required; unsupported choice rejects, corruption validated safe generic localized notice+alert, no rawkeys. Private admin locale displays target group. Exact ranges/transitions: recommended-contracts.md (D-010/012). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-010/012 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR017-01, AC-BR017-02, AC-BR017-03; US7.1/4; FR-045, FR-046, FR-047. Cases are definitions only, not executed/passed. Apply the precise D-010/012 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-018 — Operations, health and recurring work

Actors: operator, authorized diagnostic user, release reviewer. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-009/012/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Context logging, narrow token redaction, optional reporting/exporter, provider metrics/warmup and cleanup registrations exist. Narrow filtering is not proof of complete privacy redaction or multiworker safety. Service health can disclose internals.

Evidence: [telegram_bot/logging_setup.py:47–100](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/logging_setup.py#L47-L100); [telegram_bot/shared_utils.py:21–78](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L21-L78); [telegram_bot/bot.py:82–182](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L82-L182); [telegram_bot/bot.py:266–307](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L266-L307); [telegram_bot/bot.py:413–430](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L413-L430); [telegram_bot/bot.py:468–550](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/bot.py#L468-L550); [telegram_bot/user_commands.py:2095–2170](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L2095-L2170); [telegram_bot/tokens/jobs.py:12–58](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/jobs.py#L12-L58); [telegram_bot/ads_album_jobs.py:14–58](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_album_jobs.py#L14-L58).

Classification: Preserve operations; Change mandatory evidence failure and finite lifecycle (selected working recommendation for owner review).

User/data/security impact: Required bounded redacted logs/metrics/traces produced with correlation; optional exporter delivery alone maydegrade. Missing mandatory audit/outcome evidence blocks affected admission/effects, no unsafe fallbackdispatch. Alert production300s distinctdelivery/resolution. Unknownactive30d→archivedunresolved, compact365d behindbarriers. Critical RPO 0 includes latest security/privacy/policy; ordinary metadata24h excludescritical. Exact ranges/transitions: recommended-contracts.md (D-009/012/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-009/012/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR018-01, AC-BR018-02, AC-BR018-03, AC-BR018-04; US8.1/2/4; US6.1; FR-037, FR-059, FR-061, FR-062, FR-063, FR-064. Cases are definitions only, not executed/passed. Apply the precise D-009/012/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-019 — Candidate imports and schema drift

Actors: owner, migration operator, tenant administrator, affected user. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-011/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Imported-baseline migration is no-op. Later CREATE TABLE IF NOT EXISTS declarations conflict with runtime field/key assumptions for sessions, history, feedback, settings, admin context, audit, grants, ledger, examples and allowlists. Tracked declarations do not establish the actual production schema.

Evidence: [telegram_bot/alembic/versions/de64fa738e75_imported_baseline_from_existing_db.py:11–25](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/de64fa738e75_imported_baseline_from_existing_db.py#L11-L25); [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:22–184](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L22-L184); [telegram_bot/shared_utils.py:1648–1885](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1648-L1885); [telegram_bot/tokens/models.py:10–114](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/models.py#L10-L114); [telegram_bot/tokens/models.py:245–333](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/models.py#L245-L333); [telegram_bot/ads_guard.py:1538–1604](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L1538-L1604); [telegram_bot/ads_guard.py:1815–1833](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L1815-L1833); [telegram_bot/ads_pending_store.py:313–419](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_pending_store.py#L313-L419).

Classification: Approved D-001 and conditional D-011 tokens; select conditional per-entity import dispositions (selected working recommendation for owner review).

User/data/security impact: Minimal validated identities/settings/proved DM/tenant allowlists and walletledger proposedconditionalimport; current roles/superadmin bootstrap rebuild. History/context/consent/session/prompt/examples/feedbackcontent startempty/excluded. Redactedhistorical audit no current authority; pending safety NO_EXECUTE/NO_REPLAY. Actualsanitizedschemas/keys/counts/maps/authority and exactreconciliation separately approved; no fabricated events/sourcewrites. Exact ranges/transitions: recommended-contracts.md (D-011/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-011/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR019-01, AC-BR019-02, AC-BR019-03, AC-BR019-04, AC-BR019-05; US5.1–6; US1.1–3; FR-048, FR-049, FR-050, FR-051, FR-052, FR-053, FR-054, FR-055, FR-058. Cases are definitions only, not executed/passed. Apply the precise D-011/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-020 — Migration ownership

Actors: operator and release reviewer. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-011/012/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Compose launches bot after an upgrade and also offers a migration service; a shell helper supplies another entry point. Application ensure_tables is explicitly no-op. Multiple entry points are not proof of an actual concurrent production failure.

Evidence: [docker-compose.yml:170–176](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/docker-compose.yml#L170-L176); [docker-compose.yml:214–222](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/docker-compose.yml#L214-L222); [telegram_bot/tools/migrate_up.sh:1–11](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tools/migrate_up.sh#L1-L11); [telegram_bot/shared_utils.py:638–643](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L638-L643).

Classification: Change migration ownership to one canonical approved contract (selected working recommendation for owner review).

User/data/security impact: Preserve externally required data/rollback compatibility, retire competing schema authorities. Architecture/migration design remains a later separately approved plan; no actual map or run is fabricated. Exact ranges/transitions: recommended-contracts.md (D-011/012/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-011/012/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR020-01, AC-BR020-02, AC-BR020-03; US5.4/5; US8.3; FR-053, FR-054, FR-060, FR-065. Cases are definitions only, not executed/passed. Apply the precise D-011/012/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-021 — Unsafe and nonportable release inputs

Actors: operator, security reviewer and owner. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-011/012/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Tracked deployment inputs contain mutable tags, host coupling/fixed names and a credential-like assignment. Docker/dependencies are not fully resolved. Values are intentionally not reproduced here.

Evidence: [docker-compose.yml:90–113](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/docker-compose.yml#L90-L113); [docker-compose.yml:130–169](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/docker-compose.yml#L130-L169); [docker-compose.yml:227–335](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/docker-compose.yml#L227-L335); [docker-compose.yml:341–351](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/docker-compose.yml#L341-L351); [telegram_bot/tools/migrate_up.sh:1–11](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tools/migrate_up.sh#L1-L11); [telegram_bot/Dockerfile:4–26](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/Dockerfile#L4-L26); [telegram_bot/requirements.txt:3–13](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/requirements.txt#L3-L13).

Classification: Retire unsafe/nonportable release inputs (selected working recommendation for owner review).

User/data/security impact: No committed credentials, unsafe raw backups or host-specific authoritative inputs; rotated secrets/reproducible scopedDocker evidence are later release gates. Existing source remains unchanged. Exact ranges/transitions: recommended-contracts.md (D-011/012/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-011/012/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR021-01, AC-BR021-02, AC-BR021-03, AC-BR021-04; US5.3; US8.1/3; FR-051, FR-056, FR-057, FR-060, FR-065. Cases are definitions only, not executed/passed. Apply the precise D-011/012/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-022 — Mixed internal responsibilities

Actors: maintainer and reviewer; no intended user-visible change. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-010/012 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: User-command, AdsGuard and shared utility paths mix transport, authorization, data access, provider and policy operations. Module length alone is not a defect or an approved architecture decision.

Evidence: [telegram_bot/user_commands.py:1233–1396](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/user_commands.py#L1233-L1396); [telegram_bot/ads_guard.py:1538–1639](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L1538-L1639); [telegram_bot/ads_guard.py:1965–2087](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L1965-L2087); [telegram_bot/ads_guard.py:2789–2990](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L2789-L2990); [telegram_bot/shared_utils.py:679–751](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L679-L751); [telegram_bot/shared_utils.py:1228–1420](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1228-L1420); [telegram_bot/shared_utils.py:1648–1783](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1648-L1783); [telegram_bot/shared_utils.py:1999–2144](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1999-L2144).

Classification: Retire mixed internals, preserve approved external capabilities (selected working recommendation for owner review).

User/data/security impact: Constitution's modular/provider-neutral responsibility and test-first constraints apply in the later plan; no speculative service split/storage stack/framework is selected now. External preserved behavior remains explicitly tested. Exact ranges/transitions: recommended-contracts.md (D-010/012). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-010/012 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR022-01, AC-BR022-02; US1.4; all approved journeys; FR-003, FR-005, FR-006, FR-055, FR-056, FR-063. Cases are definitions only, not executed/passed. Apply the precise D-010/012 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-023 — Generated artifacts tracked as source

Actors: maintainer, build and release reviewer. Owner: project owner. Approval: historical explicit decisions retain their recorded scope; D-012/013 working recommendations selected via delegation; whole-scope approval/evidence gates pending.

Observed: Pinned tree contains 28 tracked bytecode blobs including dao/db and migration caches. This proves bytecode only, not every suspected runtime-artifact category.

Evidence: [telegram_bot](https://github.com/vahid162/ai_bot6/tree/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot).

Classification: Retire generated artifacts as authoritative source (selected working recommendation for owner review).

User/data/security impact: Source repository excludes generated credential-bearing/runtime/cache artifacts in later authorized implementation; reviewed source inputs and project-isolated builds remain canonical. No legacy files are deleted now. Exact ranges/transitions: recommended-contracts.md (D-012/013). No legacy/source/server data is changed.

Transition communication: Before later authorized rollout, show affected users/admins/operators the D-012/013 selected differences above, including removed implicit behavior, privacy/retention and safety/recovery limits. Obtain whole-spec/plan approval; mapping/evaluation/activation remain separate. Publication is not rollout permission.

Verification: AC-BR023-01, AC-BR023-02; US1.4; US8.3; FR-051, FR-065. Cases are definitions only, not executed/passed. Apply the precise D-012/013 oracles and boundary matrices in recommended-contracts.md; implementation/evaluation NOT RUN.

## BR-019 schema-pair detail

This is a declaration/query mismatch ledger, not a diagnosis of the live database. The no-op imported baseline and later create-if-absent operations mean actual sanitized schema evidence is required. None of these candidates grants import permission.

| Candidate | Tracked declaration | Runtime expectation | Required decision/validation |
| --- | --- | --- | --- |
| Sessions | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:52–60](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L52-L60) | [telegram_bot/shared_utils.py:1648–1714](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1648-L1714) | Composite chat/user/session declaration vs runtime current-session, last-activity/UI revision and chat-keyed upsert. Exclude context-bearing references; do not infer owner/topic. |
| History | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:62–71](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L62-L71) | [telegram_bot/shared_utils.py:1754–1783](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1754-L1783) | Per-message role/content declaration vs session/JSON history array. Approved exclusion; count source records without importing text/context. |
| Feedback | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:73–81](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L73-L81) | [telegram_bot/shared_utils.py:1786–1848](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1786-L1848) | message_id declaration vs bot_message_id/session_id and conflict scope. Validate actor/tenant/message and exclude content that bypasses consent/expiry. |
| Unknown items | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:165–171](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L165-L171) | [telegram_bot/shared_utils.py:1851–1885](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1851-L1885) | Question-text declaration vs session/report state plus separate listing expectations. Content handling requires D-011/D-013 approval. |
| Group settings | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:83–90](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L83-L90) | [telegram_bot/tokens/models.py:10–25](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/models.py#L10-L25) | Chat primary key/tenant field vs tenant conflict key/max-carry. Approve explicit tenant mapping and actual keys. |
| Admin context | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:92–99](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L92-L99) | [telegram_bot/shared_utils.py:1261–1285](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1261-L1285) | Composite chat/user/context-key vs user-keyed active-group/time. Stale UI state never grants current authority. |
| Admin bindings | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:110–116](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L110-L116) | [telegram_bot/shared_utils.py:1228–1258](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L1228-L1258) | Role/created-time declaration vs bound-at ordering. Independently verify current roles. |
| Audit | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:101–108](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L101-L108) | [telegram_bot/shared_utils.py:706–751](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/shared_utils.py#L706-L751) | user/action/meta vs actor/command/args/prev/new/result/reason/message fields. No fabricated identities/times; content-safe contract required. |
| Weekly grants | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:31–40](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L31-L40) | [telegram_bot/tokens/models.py:37–74](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/models.py#L37-L74) | UTC timestamp key vs week-start date. Explicit normalization must not create a second grant. |
| Ledger | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:177–184](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L177-L184) | [telegram_bot/tokens/models.py:99–107](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/models.py#L99-L107) | wallets_ledger/amount/reason vs ledger/type/ref/note, also spending lines 298–324. Reconcile approved events; do not fabricate event mapping. |
| Ads examples | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:148–154](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L148-L154) | [telegram_bot/ads_guard.py:1815–1833](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L1815-L1833) | example/notes vs text/label. Tenant/label provenance and privacy required. |
| Ads allowlists | [telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py:156–163](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py#L156-L163) | [telegram_bot/ads_guard.py:1538–1604](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_guard.py#L1538-L1604) | Global domain/user keys vs chat-scoped entries/actor. Quarantine ambiguous scope rather than exempting every tenant. |

Additional candidate evidence: [telegram_bot/alembic/versions/1a4ae0c10a8c_config_tables_bot_config_chat_config_.py:18–36](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/1a4ae0c10a8c_config_tables_bot_config_chat_config_.py#L18-L36); [telegram_bot/alembic/versions/20260105_add_ads_pending_actions.py:15–39](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/20260105_add_ads_pending_actions.py#L15-L39); [telegram_bot/alembic/versions/20260106_ads_pending_hardening.py:15–41](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/20260106_ads_pending_hardening.py#L15-L41); [telegram_bot/ads_pending_store.py:313–419](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/ads_pending_store.py#L313-L419); [telegram_bot/alembic/versions/20251111_add_token_spend_dedupe_unique.py:9–16](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/20251111_add_token_spend_dedupe_unique.py#L9-L16); [telegram_bot/tokens/models.py:245–289](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/models.py#L245-L289); [telegram_bot/dao_async.py:67–124](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/dao_async.py#L67-L124); [telegram_bot/alembic/versions/20251219_add_ads_album_cache.py:14–40](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/20251219_add_ads_album_cache.py#L14-L40). Pending processing cannot be blindly converted to retryable work, and dedupe state cannot be blindly discarded or equated with successful Telegram execution.

## Evidence limitations

No tracked measured 30-day workload/peak or benchmark report was found in the pinned source tree. Monitoring declarations prove observation capability, not utilization. No live metric endpoint, production database, server or provider was queried. Five locale selectors do not prove five complete translations: pinned fa has 170 top-level keys, en has 21, and en/ar/tr/ru share blob `5f127caf037680043562c4fbe4dfdebbc8861fd4`. This is source-catalog evidence, not the final launch-key manifest. BR-023's 28 bytecode blobs are tree evidence, not fabricated binary line citations. Approval/evaluation gaps are tracked in the decision register and measurement contract.
