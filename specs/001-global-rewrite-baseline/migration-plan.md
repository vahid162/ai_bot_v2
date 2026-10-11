# Architecture migration and handover plan

DRAFT / NOT APPROVED; future procedure, not operational authorization.
ai_bot6 source/GitHub/deployment remains immutable. No server connection, file change,
snapshot, freeze, bot switch, import or migration occurred.

## 1. Evidence before actual mapping approval

Static legacy source drift is real, actual deployed schema isUNKNOWN. Token runtime
mentions week_start_date/max_carry/ledger while historicalmigration uses
week_start_utc/wallets_ledger and differs in carry columns.
[Runtime model](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/tokens/models.py),
[historical migration](https://github.com/vahid162/ai_bot6/blob/cdb0efd4b39eaafd1265822350606887fe3b40b3/telegram_bot/alembic/versions/990cbca38343_init_wallets_grants_ledger_from_001_.py).
This proves a risk, not which table production actually uses.

Owner-controlled approved sanitized evidence must identify actual deployedrevision,
catalog/types/keys/constraints/indexes/timezone, appliedAlembicrevisions+checksums,
counts, tenant/userbindings, sourcewriter/receiverownership, wallet/events/current week
grant/capskip/failedspend/unknownresult meanings. Publish only suitably redacted
surrogate evidence in GitHub. No productioncredentials, rawIDs/maps/messages/dumps.

Approve field-level mapping/disposition/reconciliation before import or migration
plan approval for the affected scope. Missingkeys/times/roles are not inferred.
Source snapshots must be stable and auditably watermarked; later sourcechange
invalidates that mapping/rehearsal input. No current snapshot is available.

## 2. Clean target schema and future upgrades

New reviewed target contract/data-model is authoritative, not copied legacyDDL.
Alembic is the only production schema mechanism. Two isolated environments,
METADATA and CONTENT, each one deployable head; one coordinator/release manifest
declares exact compatiblepair/checksums. It acquires orderedperstore advisorylocks,
preflights capacity/compatibility, records before/afterrevision/outcome and exits
before dependent incompatible appversions start. Runtime onlychecks compatibility.

No runtime create_all, conditionalmissingcolumn fallback, manualDDL, alteredapplied
revision or secondstartup migrator. Partialpaired-storemigration leaves dependent
capability unready; retries require verified priorjobstop/currentlock and explicit
recovery, not an expiredlease assumption.

Everychange describes expand→boundedidempotentbackfill→validate→appswitch→contract,
supporting reviewedold/newV2 versions through an explicit compatibilitywindow.
Single-storeconstraint/index changes require measuredlock/statementlimits.
Destructivecontract requires separateapproval and testedforwardrecovery.
Autogenerate proposes diffs only; review renames/CHECK/RLS/triggers/views/indexes.
[Autogenerate](https://alembic.sqlalchemy.org/en/latest/autogenerate.html).
Supported predecessor set: firstrelease empty→head only; laterreleases enumerate
actual deployedcompatibleV2 revisions. OldAlembicgraphs are not falsely supported
upgradepaths into a clean newdatabase; legacydata enters versionedimport.

## 3. Import and reconciliation rehearsals

Use approved synthetic/irreversiblyanonymized fixtures in CI and separately authorized
restricted sanitized mapping evidence for real rehearsal. Follow [manifestV1](contracts/import.md).
Minimize before persistence; stagingencrypted<=7d; one exactdisposition perrow.
No history/context/oldconsent/session/prompt/examples/secrets. Currentrolesreverified;
new consentOFF and examplesempty. Import pendingeffects as non-executable safety
history/NO_REPLAY only, never runnablejobs.

Wallet reconciliation is exact pertenant/user and total: proved originalbalance,
immutablelinked sourceevents, legitimate above-capbalance, current weekgranted/skipped
proof and all terminal paid/externalsafetyoutcomes. Checkpoint is NON_EVENT with
independentcounts/hash/provenance, never balancingdelta. No retrorefund/rewrite or
absentrow=unpaid/grantmissing shortcut. Redactedhistoricalaudit is not currentauthority.

Rehearsal acceptance:100%rowsaccounted,0unexplainedfinancial/safety differences,
repeatimport0new effects/events, approvedP1/authorization/privacy/race casespass,
restore/rerun/purge handling verified. Quarantine is not success; scopecannot silently
shrink to exclude an unexplained affectedwallet. No rehearsal executed.

## 4. Recovery and rollback selection

METADATA RPO0 proposal covers one independent primarydomain loss/partition with
surviving synchronizedcopy and verifiedfencing/currentacknowledgedcheckpoint.
No async/localfallback. Backups<=30d METADATA-only; dailyordinaryRPO<=24h is not
critical RPO0. Restore must recover complete latestcriticalproof, currentdenials/
rolesrevocations/consentclear/deletionactivation/unknownholds and sourcefences before
exposure. Missingproof→unready/failclosed, lostconsentOFF. Purgeexpiredordinarydata
first. CONTENT is deliberately not restorable; disclose losthistory/nonpersonalexample
version impact and disable dependent deletion iftestedinputs unavailable.

Rollback before V2 effects: keep V2 trafficgated, no changes to oldsource.
After first V2 effect/ledger/denial/consent/grant outcome: select rehearsed compatible
V2 image over SAMEcanonicalmetadata/fences, or no-effect safe state. RestoringstaleDB
or simply resumingoldai_bot6 would lose newcriticalstate and is NOT safe rollback.
No priorV2 release exists yet: designate and rehearse initialrollbackartifact before
firstcutover; cannot invent one. Rollback-to-legacy is not selected/authorized and
would need new actualreversemaps/reconciliation/authority.

Combinedrestore+cutoverrollbackdrill<=60min; independentrelease/rollback<=30mineach,
originaloperator-go→verifiedreadiness including reconciliation/trafficgate. No timed
success or passingdrill exists. Destructive/unsafe downgrade uses reviewedforward
recovery, not a pretend reversiblemigration.

## 5. Future cutover gate — no old deployment mutation authorized

ConstitutionI explicitly forbids changing olddeployments. A real freeze/receiverstop/
writertransfer therefore requires owner-controlled handoverevidence and new scoped
authority consistent with that rule, or a separately reviewed constitutional
clarification. Drafting this section does not grant either. Until resolved, cutover
is BLOCKED, not “run stop on ai_bot6”.

Future handover must prove stable finalwatermark and one receiver/walletwriter at a
time; newpolling starts only after reviewedrouteownership. No automaticwebhook deletion,
dropupdates/tokenrotation/epochreset as a shortcut. Preserve admittedsourceidentity,
latestcriticalcheckpoint/current week/unknownreview fences; imported pre-fence work
cannot be reclassified asfresh just because age<=24h.

Activation preflight: approvedGitHubrelease+spec/plan, reviewedactualmaps/sourcecounts,
rotationproof, maintainedcompliantprovider, current SA bootstrap/roleverification,
P1/security/locale/Docker/migration/race/reference/recoveryproof, pricedfaultdomain
and approvedcostlimits, testedabort/rollbackartifact, separate server/handoverpermission.
History/contextfresh;consentOFF;deletion OFF absent separateowneractivation.

Abort criteria: any unexplained count/balance/safety mismatch, dualreceiver/writer,
lostrequired evidence/standby/fence, unapprovedprovider/region, privacy/authorization
failure, unresolved partialschema, unprovedrollback, breached approvedtiming/capacity.
Gate affectedtraffic and preserve actualoutcomes; never replayunknowns to “complete”
cutover. Owner/alternate and checkpointdecision must be recorded.

## 6. Post-cutover — later authorization only

Exact wallet/period/terminaloutcome/sourcefence/currentcontrol reconciliation,
single-ingressownership, capabilityreadiness, scopedunknownassignment and alertchecks.
No automatic oldhistorycopy, topup/refund, corpusactivation or sourcecleanup.
GitHub remains sourceoftruth; server may later fast-forward reviewedcommit with
localchanges preserved, never force-reset or unrelatedservicechanges.
