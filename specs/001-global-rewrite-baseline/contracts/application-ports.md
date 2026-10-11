# Application ports V1

Design contract only, no interfaces/code generated. Domain/application imports no
Telegram, Flowise, SQLAlchemy, HTTP, monitoring or deployment SDK. Versioned immutable
DTOs contain value objects/typed categories, not framework exceptions/session objects.

| Port / DTO | Fields and semantics |
| --- | --- |
| AuthorityPort / AuthorityDecisionV1 | original actor/kind,tenant/topic,action,target,current source revision,verified_at; ALLOWED/DENIED/UNAVAILABLE. Current reads/writes/dispatch all recheck. |
| ConfigPort / EffectiveConfigV1 | explicit scope,validated values,source precedence,monotonic revision,activation constraints; invalid security values fail closed. |
| IngestionPort / InboundEnvelopeV1 | schema_version,correlation/work ID,botnamespace/source epoch,update_id,original logicalID/time,actor/scope/route,latest edit revision,volatile payload capability. Durable serialization strips rawtext/framework data. |
| ScopeBindingPort / ScopeBindingV1 | original actor/scope/topic/action/target/prompt or answer ID,nonce/revision,expiry; no current selected-group retargeting. |
| ConversationPort / ConversationRequestV1 | own scope,locale,transient necessary input,eligible own context/source-provenance,current generation,absolute deadline,reviewed provider config ID. No arbitrary endpoint/credential/session carryover. |
| AnswerProvider / ConversationResultV1 | bounded transient plain answer,typed failure,provider config/result revision; discard unapproved echoed context/tools/diagnostics. |
| Classifier / ClassificationRequestV1 | transient eligibletext/caption,approved bounded fewshots,current policy/modelrevision,deadline; no binary/OCR/URL retrieval. |
| Classifier / ClassificationResultV1 | AD/NOT_AD/UNKNOWN,finite [0,1]score,typedreason,reviewedrevision; malformed/boolean/string/NaN/missingfields→UNKNOWN. |
| HistoryAccessPort / EligibleContextV1 | owner/scope,current consent+generation,all source owners/expiry/generation,<=20own entries/16000scalars; no another-owner storage without every source consent. |
| TokenPort / GrantResultV1 | exacttenant+nameduser+current UTC week,GRANTED/SKIPPED_CAP/prioroutcome/ineligible; only explicitbalance/scheduled triggers. |
| TokenPort / PaidSettlementV1 | trustedsource identity,confirmedwarningreceipt,currentwarn/switches/eligibility,outcome/event ref; every settledidentity terminal. |
| RequiredEvidence / AuditAppender | boundedredacted attempt/admission and result,current scope/correlation,chain/checkpoint identity; failureblocksaffectedwork before privilege/dispatch. |
| DurableWork / JobClaimV1 | work ID,owner,current fencedgeneration,claimexpiry,originaladmission/due clocks,safe disposition; no leaseexpiry replay permission. |
| EffectDelivery / EffectIntentV1 | stableeffect/parent/chunk/memberidentity,exacttarget/action,current gates,due/deadline/fence,template metadata or volatileanswerreference; no raw durable answer. |
| EffectDelivery / EffectReceiptV1 | attemptID/exacttarget/category,confirmedexternalIDs or result,times,noncontentevidence; unknownneverassumedsuccess/failure. |
| LocalizationPort / LocaleResolutionV1 | effective fa/en/ar/tr/ru,precedencesource,validatedcatalogrevision; neverrawkeys. |
| Import/Schema/Recovery ports | versioned manifest/revision/checksums/dispositions/reconciliation/approval; runtime readcompatibility only, noDDL/autorepair. |

Application use cases own transaction boundaries. Cross-context transactions enlist
repositories through an opaque UnitOfWork and intent methods; concrete sessions live
only in infrastructure. Contexts cannot select each other's tables. Content-store
publication is staged/idempotent, not fictitious atomic commit with METADATA.

Public event schemas include eventversion/ID/correlation/sourcecontext/scope/occurred_at/
causation/currentrevision and bounded noncontent payload. New versions require reviewed
compatibility; no generic free-form event bus or persisted private text. At-least-once
internal delivery plus exact idempotency, never an external exactly-once claim.

External outcome vocabulary: NOT_SENT_PROVEN, REJECTED_PROVEN, APPLIED_CONFIRMED,
UNKNOWN_MAY_HAVE_EXECUTED, PARTIAL_CONFIRMED_UNKNOWN; safe INPUT_UNAVAILABLE,
EXPIRED, REVOKED, UNSUPPORTED. UNKNOWN stops and creates scoped durable review.
Repository COMMIT_UNKNOWN is distinct from transport failure and reconciles same keys.

All ports define finite timeout, cancellation and concurrency in operations.md.
Implementations follow test-first only after approved plan and separate implementation request.
