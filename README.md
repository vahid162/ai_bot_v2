# ai_bot_v2

Standardized rewrite of the legacy Telegram bot. Application implementation has
not started: the behavioral specification and the later architecture/migration
plan both require explicit project-owner approval.

## Source of truth and workflow

- This GitHub repository is the authoritative source and the only write target
  for current work. Do not change WebMaster files, checkouts, containers, services,
  or data, including `/www/dk_project/dk_app/guardbot`. Server synchronization and
  deployment are paused until the owner explicitly authorizes them again. Any
  later authorized synchronization must consume a reviewed GitHub commit, not
  unpublished file copies.
- `vahid162/ai_bot6` is an immutable, read-only behavioral reference. Do not
  modify its source, GitHub metadata, or live deployment.
- Current constitution: [.specify/memory/constitution.md](.specify/memory/constitution.md).
- Current draft specification: [specs/001-global-rewrite-baseline/spec.md](specs/001-global-rewrite-baseline/spec.md).
- Readiness: [requirements checklist](specs/001-global-rewrite-baseline/checklists/requirements.md).
- Next: resolve specification questions and evidence coverage, obtain owner
  approval, prepare the architecture and migration plan, obtain plan approval,
  then generate tasks and implement. Publishing a draft is not approval.

The owner has selected an empty conversation history for the first cutover.
Legacy conversation content and historical conversational context are not imported; old data
is not deleted. Other entity migrations remain subject to separate approval.

Only after new server authorization, synchronize a clean checkout using Git fetch
and fast-forward-only updates; preserve local changes before updating. Do not use
forced resets or replace unrelated server services.

The tooling commands below are documented for future authorized use; do not run
containers or prepare the image on WebMaster during the current GitHub-only work.

## Project-local Spec Kit

Specify CLI 0.16.4 runs in this project's isolated, temporary Docker container;
there is no host-global CLI installation. Existing Spec Kit files must not be
reinitialized or upgraded as part of specification review.

```sh
./specify version
./specify --help
```

See [tooling instructions](tools/specify/README.md) for image preparation and
isolation. Internet-dependent CLI commands are intentionally unavailable at
runtime.

The current feature is `specs/001-global-rewrite-baseline`. The ignored
`.specify/feature.json` is a per-checkout pointer, not a shared deliverable. To
resolve the feature without relying on that local pointer:

```sh
SPECIFY_FEATURE_DIRECTORY=specs/001-global-rewrite-baseline \
  .specify/scripts/bash/check-prerequisites.sh --json --paths-only
```
