# Legacy import manifest V1

Design only; actual catalog/rows/mapping/snapshot are UNKNOWN. No export, production
access, migration/import code or new ledger event. Legacy source remains immutable.

Future restricted evidence manifest fields: source repository+commit, owner-provided
deployment schema/appliedrevision/checksums, stable sanitized snapshot watermark,
sourceepoch/fence, mappingrevision, entityscope, rowcount/checksum, validation/
reconciliation approvals, batchidentity, targetrelease/revision pair and expiry.
Published GitHub reports use synthetic surrogate references, never production IDs,
rawmaps/messages/secrets. Identified staging is not a CI fixture.

Each candidate sourceidentity has exactly one IMPORTED_UNCHANGED,
TRANSFORMED_IMPORTED, REJECTED, QUARANTINED or EXCLUDED disposition. Report exact
counts byentity+tenant, original/target keybindings, transformationreason, integrity
errors and unresolveddifferences. Independently sum counts and verified financial
totals; no invented balancing grant, missingrow=grant assumption or dedupe=paid proof.

The selected D-011 table is authoritative: minimal validated users/tenants/settings/
explicitallow-deny/tenantallowlists and proved wallet/immutableledger/currentperiod/
safetyoutcomes are conditional. Freshroles/current SA bootstrap reviewed separately.
Oldhistory/context/session/panels/prompts/consent/feedbackcontent/examples/secrets
excluded before persistentstaging; pendingeffects safety-only/non-executableNO_REPLAY.
Invalid/ambiguousschema/time/ownership/meaning rejects/quarantines, not guesses.

Re-run same batch/map/sourceidentity idempotently; promotion requires exactversioned
approval and complete scope reconciliation. Unresolved wallet/safety mapping blocks
affected cutover; quarantinedpayload expiry does not make it reconciled.
Staging encrypted/minimal<=7 days, quarantinepayload<=30 days, redactedreport365days.
No sourcecredentials/key material or rawlegacy conversational context is retained.

Reference validation scenarios: clean empty target, repeatedbatch, sameweekgrant/
SKIPPED_CAP, insufficientoldpaymentrecord, above-capwallet, orphan/timestamp/type/
tenant ambiguities, oldunknownexternal effects, partialpromotion recovery.
All are future testsNOT RUN. See [migration plan](../migration-plan.md).
