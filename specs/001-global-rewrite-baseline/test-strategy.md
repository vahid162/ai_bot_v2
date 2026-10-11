# Test architecture and requirement coverage

DRAFT / NOT APPROVED. No executable tests/fixtures/code/migrations/Dockerfiles,
provider/bot calls, benchmark, gold evaluation or drill written/executed here.
Every result below is NOT RUN. Document completeness is not behavioral PASS.

## Test-first sequence and coverage

After explicit owner plan approval and a separately requested tasks/implementation
workflow: establish synthetic/irreversiblyanonymized characterization oracle,
write acceptance/contracttests and observe the intended red failure before governed
implementation. Changes/retirements use approvedD-001–013, not oldunsafebehavior.
Defectfixes start with a reproducer; refactor undergreen. No unapprovedbehavioromission.

[Behavior-ledger](behavior-ledger.md),[acceptance-cases](acceptance-cases.md),
[spec](spec.md),[measurement-contract](measurement-contract.md) are bindinginputs.
23families/65FR/14SC/93supplementarycases are documented counts, not executablecoverage.
Future trace manifest links EACH BR/FR/SC and discovered edge totestID/level/oracle/
fixtureversion/locale/scope/owner/result/evidence. Unknownlegacybehavior is logged/
characterized, not silentlypreserved. Currentqualitychecklist16/16definitions is
not passingcode, gold or migrationevidence.

| Record | Required coverage focus |
| --- | --- |
| BR-001 discovery/menus/health | Scope-specific menus, identity, redacted capability health. |
| BR-002 private/ask/chunks | Unified inputs, feedback, safe ordered output and partial UNKNOWN. |
| BR-003 group/cleanup | Every mode/route/current actor, retired member cleanup. |
| BR-004 sessions/history | Consent, generations, owner export/clear, minimum source expiry. |
| BR-005 feedback/unknowns | Confirmed exact answer/topic, immutable votes, scoped review. |
| BR-006 DM/users | Explicit deny, confirmed unblock, independent rights, minimal paged listing. |
| BR-007 management/prompts | Original actor/target binding, expiry, no AI fallthrough. |
| BR-008 roles/audit | Fresh scoped authority, anonymous restrictions, required evidence. |
| BR-009 scoped settings/lang/menus | Validated revisions, current selected target, exact reachable routes. |
| BR-010 classification | Finite schema/threshold, exclusions, provenance and UNKNOWN. |
| BR-011 modes/guards/presets/mute | Current mode, no structural-only deletion, correction, no hidden mute deletion. |
| BR-012 pending/albums | Original clocks, member oracle, stale leases, no UNKNOWN replay. |
| BR-013 examples/allowlists/diagnostics | Tenant provenance, exact hosts, bounds, confirmation, no raw text diagnostics. |
| BR-014 wallets/grants/carry | Current-week triggers, terminal cap-skip, above-cap retention. |
| BR-015 spending/ledger | Confirmed warning-only charge, all terminal identities, exact atomic races. |
| BR-016 configuration/seeding | Typed DB precedence, security fail-closed, no guessed authority. |
| BR-017 localization | Five complete catalogs, Unicode/RTL/fallback and no raw keys. |
| BR-018 operations/jobs | Health, mandatory evidence vs optional export, alerts, recovery clocks. |
| BR-019 imports/schema | Actual approved maps, exact dispositions/totals and clean schema contract. |
| BR-020 migration ownership | Negative startup/racing-migrator tests; dedicated single job. |
| BR-021 release inputs | Negative mutable/host-coupled/secrets tests; reproducible bounded images. |
| BR-022 responsibilities | Negative outward/private imports and cycles; module decomposition review. |
| BR-023 generated source | Negative generated artifacts/secrets/cache/dump/build-context tracking. |

Every FR-001 through FR-065, SC-001 through SC-014 and individual acceptance case
needs its own future trace entry and evidence; this design table alone never earns PASS.

## Layers and realistic dependencies

- Pure domain/application: fakeUTCclock, deterministicIDs/nonces, exactmode/exemption/
  eligibility/gap/deadline/role/consent/locale/token states, inwarddependency checks.
- Adapter contracts: boundedTelegramupdates/commandnames/panels/topics/anonymous/
  responsecategories/chunks; providerfiniteenum/schema/privacy gate; intentrepository/
  configs; cancellation/timeouts/phaseproof. LiveAPI behavior not inferred fromfakes.
- RealPG 18 integration: canonical schema/constraints/FKs/RLS/per-tasktransactions,
  scopepoolreset/deadlocks/commitunknown, claims/stalefences/required-evidence faults,
  cross-storeclear/publish/restart and wallet/grant concurrency. SQLite/mockSQL invalid
  for theseproofs.
- Migration: clean→headpair and eachsupportedpriorV2revision; immutablechecksums/
  schema-queryagreement; single coordinator/concurrentjobfence; failedpartialresume/
  compatibility/backfill; actualsanitizedapprovedmapping and exactreconciliation.
- CriticalE2E: allapprovedP1journeys,5locales,security/privacy/modes/ledger/metadata-only
  recovery and resourcefaults usingfakes plus disposablePG/Docker. No livecredentials.
- Security/Docker/supply: threatsR01–15, effectiveUID/caps/RO/tmpfs/limits/network/
  gatewayDNSescape/secrets/context/locks/SBOM/provenance/scan; contentkey/WALremnant
  erasure proof; expired/tombstoned restore can't resurrect permission/text.
- Recoveryfaults: crashbefore/afterDISPATCHING/receipt/COMMIT, bothstores separately,
  synchronouscopyloss/partition/oldwriterfence, incompletealbum/partialanswer, mandatory
  versusoptionaltelemetry, archivedunknown/compactpurge barriers. Allsinkloss FAIL.
- Nativefirstuse:50participants,10eachlocale(5regular+5admin),>=45overall and>=9perlocale;
  declaredrolecohorts/assistance/abandonment; no fictionalparticipants orresults.

## Deletion-safety oracle

Existing30AI-draftedseeds+12structuralvariants+4historyfixtures are NOT owner-approved
gold. Freeze held-out>=1000 independentlylabelledadjudicateditems,>=100eligibleAD+
100legitimatenonexemptNON_AD perfa/en/ar/tr/ru plus exemptions/UNKNOWN/structural/album
variants;trainingseeds disjoint. Ownerapprovesactualcoverage/version/labels separately.

Proposeddeletionevaluation uses hypotheticalvalidallgates toavoidvacuousallOFFPASS;
zero falselegitimatelogical/memberdeletion, correcteligiblepositivefunctionalcases,
exempt/insufficient/unknownprotected,<=once effects. DeletionactualactivationOFF.
Passingtest is necessary, not explicitowner approval toactivate orguaranteezero realerrors.

## Frozen reference and clocks

Use exactselectedStandard and2× measurement-contract,3independentrunseach,10minwarmup/
30minmeasure/5mindrain, predeclared category/locale/tenant/actor/topic/identity/time/
payload manifests and counted offered/accepted/rejected/duplicate/unknown/late/
safe/results. 10800unique+2160dups versus21600+4320. Everyhealthyvalidreference
input fits ordinaryquotas/gaps; no unexpectedreject/lostacceptedwork. Fault/negative
runs declaredseparately, never posthoc exclusions. Resourceoperationmanifest includes
all nine components inoperations.md, same<=4CPU/8GiB atbase+2×.

ControlledAI2s/classifier.5s/transport.05s,200ops/sfakebot/10sfakechat;oneanswerchunk
<=1000UTF16. Actualquotas/provider/DNS/independenthostloss require separate evidence.
No inferredTelegramreceipt, healthyfast-errors-onlypass, extra-drainontimepass or
hidden dependencycapacity. SC003unknownreview isn'tanswer/safeterminal, butSC011
boundedrecordedoperatorreview is avalidrecoverydisposition.

100independenttrials×20actors grant/debit exactreconciliation. Restore+rollbackcombined
<=60min/operatorgo→verifiedreadiness; release/rollback<=30mineach. CriticalRPO0
proves survivinglatestacknowledgedfence/checkpoint underapprovedfaultenvelope; dailybackup
notproof. Alertproduction<=300s excludeshumanresolution butnot missingevidence.

## CI and release gates

Format/lint/typecheck, no architecturecycles/crossprivateimports;500logicalline
decompositionreview;>=80%applicationlinecoverage,>=90%criticalbranch(authority/
tenant/ledger/migration/Adsactions). Secret/static/dependency/license/security/
contract/PGmigration/Docker tests blockmerge. Criticalbaselineexclusions needapproved
time-boundedexception. Flakytests defectowner/expiry/link, no rerun-untilgreen.

Future release includes exact reviewedcommit/task/requirementtrace, test results,
security/privacy/schema/lock/imageimpact, namedowners/alternates, rollback/runbooks,
frozenmanifest and approval. No metric screenshot or count substitutes for rawredacted
timed evidence. Actual execution results remain NOT RUN in this draft.

## Independent design review record

Three read-only reviewers checked transport/provider, storage/migration and
security/delivery. Clarifications integrated: witnessed publication before privileged
processing/all dependent effects, source-proved-only historical ledger snapshots,
new critical writes gated after failover until sync redundancy is restored, and
pre-decode bounded wire/decompression/JSON responses, and complete warning-debit prerequisites. This is document review,
not test execution, owner approval or evidence-gate closure.
