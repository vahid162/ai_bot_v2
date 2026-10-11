# Future validation guide — no execution now

This is Phase1 design guidance, NOT a runnable implemented project.
Application entrypoints, production Compose, migration/import scripts and test commands
do not yet exist. No installation/container/servercommand is run in this planning turn.

## Preconditions

1. Read [approval-record](approval-record.md) and [owner review](plan-review.md).
   Spec53b8c117 approved; plan notapproved. ResolveG-01–05 factual/design gates and
   obtain explicit plan approval before separately requestedtasks/test-first work.
2. Futureimplementation mustcreate reviewedsource/locks/config/tests and publish to
   authoritativeai_bot_v2 first. Never write unpublishedcopies toWebMaster orai_bot6.
3. Validation uses a disposableisolated project with synthetic/irreversiblyanonymized
   data; no liveserver/Telegram/AI credentials, IDs/maps/backups. Production/handover
   needs newauthority evenafter code exists.

## Intended future validation order

- Resolve exactreviewedGitHubcommit and releaseinputidentitymanifest; verify hashes/
  digest/SBOM/provenance/licenses/scans. Existing projectSpecCLI remains separate,
  no hostglobalinstall orreinitialization.
- Render/lint futureprojectscopedCompose; verify complete evaluatedresourcebudget,
  dedicatedvolumes/networks/roles, nonroot/caps/RO/secrets and fixedegress policy.
  Profile names cannot substitute forcredential/approvalcontrols.
- Bring up disposablePG 18 metadataprimary+standby and isolatedcontent only in the
  authorizedvalidationenvironment. No publicports/sharednetworks/livecredentials.
- Run dedicatedmigrator, prove cleanrevisionpair, singlejobfence/checksums, compatibility
  and realquery/RLS/schema tests. Runtimeentrypoint nevermigrates.
- Observeacceptance/contractred before production implementation; then unit/integration/
  E2E/security/Docker/migration/concurrency/locale greenproof and coveragegates.
- Run exactfrozenbase+2× references, each3times, allcomponents/bounds/denominators
  declared; separatelyrunfault/partial/unknown/clear/expiry/restore/rollback cases.
- Review independentlyapprovedgold/evaluation; leaveautomaticdeletion OFF unless
  separateexplicitmatchingowneractivation exists.
- Collect redactedtrace/reconciliation/alerts/drill evidence and approvalstatus;
  missingfacts/failedresult remain FAIL/NOT RUN, not success by skippedtests.

Commands will be documented from actualimplementedreviewedentrypoints under approved
tasks; placeholders here must not be mistaken for existing scripts. Content/provider/
gateway/recoveryproofs must close their relevant gates before persistence/integration/
release. Nothing in this guide authorizes server synchronization orolddeployment
freeze. Endthisplanningworkflow beforetasks/source/tests/serveroperations.
