# Security, Docker and operations design

DRAFT / NOT APPROVED. All settings are proposed design ceilings, not current server
configuration. No Docker/YAML/image/build/secret/network/server action performed.

## Topology and isolation

One runtime image, roles ingress and metadataworker. Three PGprocesses: metadata
primary+independent-domain synchronous standby, physically separatecontentprimary.
Project-internal metadata/content/egress-client networks; fixed-purposeegressbroker
alone joinsoutwardnetwork. Independentobserver/checkpointwitness and future
backup/reconciliation tooling have least-privilege scopedaccess. No arbitraryCONNECT,
publicwebhook, hostnetwork, sharedexternalnetwork, Docker socket or globalcontainer_name.
Docker internal network is not DNS/host allowlist.
[Compose networking](https://docs.docker.com/compose/how-tos/networking/).

Gatewayselection/effectiveDNS+addresscontrol remainsG-04: approvedfixedauthority/method/
path; HTTPS host/SNI/certificate verification; no redirect/arbitraryports/userinfo/
URLinput. Denyloopback/private/linklocal/multicast/forbiddenIPv6 and rebinding at actual
connection; separatelyapproved exactprivateproviderintegration only, not blanketprivate
egress. No rawbot-tokenURLs/requestbodies/accesslogs. No customgatewayimplementation
or privilegedhostfirewall is authorized by this document.

Distinctbuildtargets/profiles runtime/migrate/ops/projectSpecKit; profiles aren't
authorization. RuntimehasnoSpecifyCLI/DDL credentials/compiler/toolchain. Migration
hasnoBot/provider token/publicegress. Ops roles separatelyapproved. Existing project-local
spec tooling is not upgraded/reinitialized or globallyinstalled.
[Multistage](https://docs.docker.com/build/building/multi-stage/),
[profiles](https://docs.docker.com/compose/how-tos/profiles/).

Containers fixednonrootUID/GID, cap_drop ALL, nonewprivileges, read-only root,
revieweddefaultseccomp, no privileged/devices/unrestrictedbindmounts. Projectnamedvolumes
with verifiedownership/encryption/capacity; no root-chownentrypoint fallback. App tmpfs
<=32 MiB each, PGtmpfs<=64 MiB each/sharedmemory<=128 MiB each countedincgroup. Health/
metricsinternalonly; nopublishedruntimeportsneeded. Endpointstartup/alive/ready
redacted and bounded; no secret inchecks. ExactUID/GID/imagepolicy verifiedfutureCI.

## Bounded reference allocation

| Evaluated component | CPU equivalents | MemoryMiB |
| --- | ---: | ---: |
| Ingress incl transient AI/classifier | 1.00 | 1536 |
| Metadatajobworker | 0.25 | 512 |
| METADATAprimary | 1.00 | 2048 |
| CONTENTprimary | 0.50 | 1024 |
| METADATAstandby | 0.50 | 1024 |
| Egressbroker | 0.125 | 128 |
| Observer/optionalcollector/checkpointwitness | 0.125 | 256 |
| Controlleddependencyfakes | 0.25 | 512 |
| Measurementharness | 0.25 | 256 |
| **Total** | **4.00** | **7296** |

Heap/tmpfs/sharedmemory/queues count; no hiddenremote dependency resources. If gateway,
erasurekeyservice,witness oralertstorage requires additional evaluatedresources,
declare/rebalancewithin4CPU/8GiB and rerun frozenbase+2×, not unreportedextra capacity.
Samehostteststandby is not independent production domain proof. No measured PASS.

ProposedPIDs ingress128/worker64/eachPG256/others64; nofile ingress2048/worker1024/
others1024. Rotatecontainerlogs10MiB×3/service, redactedretention14d; auditnotlogs.
Gracefulappstop30s, PG60s proposed; stopadmission, persist dispositions, never repeatunknown.
Volume/WAL/free-spacequotas and retention evidence must appear in frozenmanifest;
Compose namedvolumes do not themselves enforce bytequota. Capacitypressure stops
affectedadmission beforeaccepted eviction, unboundedWAL growth orsyncfallback.

Asyncingressproviderconcurrency64answers+32classifier, separateHTTPpools64+32;
Telegramoutboundpool32,pollpool1. Metadata DBpools ingress20+worker10+observer2;
contentpools ingress4+worker2; PGmaxconnections60 proposed with reservedops capacity.
Per-taskAsyncSession; no DB transaction across network. DBconnect/poolwait1s,statement3s,
transaction5s ceilings; synchronouscommit waitdeadline5s→unready/COMMIT_UNKNOWN
reconciliation, never session setting downgrade. Normalquery latency must fitSLO.

Answerouterdeadline30s(connect3/write3/read20/pool.25), classifier10s(3/3/5/.25),
Telegrammethod15s(3/3/10/.25), polling20slongpoll/read25/outer30s. HTTPphaseceilings
do not define totaldeadline; cancellation propagates with typedattemptphase/evidence.
TLSon,redirectsoff,HTTP1.1,noimplicitenvironmentproxycredentials. Originalrunnable
queue120s, scheduledclockstartsdue; retries/lease/progressdonotreset.
External<=3attempts/30s onlyprovennonexecution, wait1s/3s+positive0–20%jitter; honor
RetryAfteronlyifdeadline/budgetfit; UNKNOWNnoretry. Database-only transientretrymax3
within5stotal with jitter; uncertainCOMMIT requires canonicalidentityreconciliation.
Polling safe-read threeboundedretries1/2/4s then30scooldown supervisedcycle;401/409/
leaseloss stops ingressunready. Theseceilings aren't healthycapacity proof.
[HTTPX timeouts](https://www.python-httpx.org/advanced/timeouts/),
[limits](https://www.python-httpx.org/advanced/resource-limits/).

## Data, authorization and secrets

Scopedrepositories plus PG RLS defaultdeny; runtime notowner/superuser/BYPASSRLS,
noDDL/TRUNCATE. FORCE RLS/read+writechecks, transaction-local validatedscope and pool
rollback tests. GlobalSA uses distinctauthorizedports, not “alltenants” selector.
RLS doesn't replace actualcurrentgroup authority or protect compromisedappscope.
[PG RLS](https://www.postgresql.org/docs/18/ddl-rowsecurity.html).

Secretrotation/runtimeinjection only, service-specificreadonlyfile/store grants.
Composefilemounts aren't encryptedsecretmanager; readability/hoststoragepermission
mustbeproven. No committedenv/buildarg/label/CLI/log/healthsecret. Any historically
committedcredentialtreatedcompromised; ownerrotationproof before integration, never
copied. Currentplan readsnoactualsecrets.
[Compose secrets](https://docs.docker.com/compose/how-tos/use-secrets/).

Metadata-onlyencryptedbackup<=30d, independentlydurablesyncstandby, currentcritical
fencing/checkpoint proof. Content noarchive/backup/replica; erasurekey/storage lifecycle
G-03 before persistence. All reads/exports/context check owner/allsourceconsent/
generations/minexpiry. Processcoredumps/diskspill/rawdiagnostics disabled; kernel/host
swap/snapshot exclusion require futureverified deploymentcontract, nothostchangesnow.

Audit: append-onlyredacted canonical chain/signingkeyversion, keyoutsideDB. Independent
project-scopedappend-onlysignedcheckpoint witness observes everycriticalbatch before
acknowledgment/privilegedexternaldispatch. Witnesscredentialretention separated;
witnessunavailableblocksrequiredaffectedadmission/dispatch. Commit-before-witness
failure remains recorded/unacknowledgedpendingreconciliation, not inventedrollback.
Chain alone doesn't detect undetectedtailremoval or fullycompromisedsigningapp.
Witnessprotects evidence, doesnotrepair ledger; exactdeployment/storage identity
and all-inresource/cost needG-05 review.

## Threat and legacy risk register

All risks OPEN; owner names/alternates and evidence links assigned before plan approval.
A control described here does not close a risk. Severity, responsibility role,
remediation and required validation:

| ID / risk | Severity / owner role | Control and closure evidence |
| --- | --- | --- |
| R01 staleSA/admin/DMdenybypass | Critical/security | fresh originalactor/targetscope everyroute/read/write/dispatch; denyoverride; revoked/stale adversaries. |
| R02 cross-tenant/topic/owner leak | Critical/data | tenantqualifiedkeys/ports/RLS; ownerexports/sourceconsent; isolation/pooledscope tests. |
| R03 callback/prompt/deeplink tamper | High/identity | opaqueoriginalbinding/expiry/CAS/currentreauth; forged/stale/crossgroup tests. |
| R04 model/provider exfiltration/storage | Critical/provider | untrustedoutput/noauthority/tools/URLs; Flowise G-01 agreement+technical proof, no rawtextlogging. |
| R05 SSRF/egress | Critical/platformsecurity | fixedgateway/noarbitrarydestinations/redirects/DNSIPpolicy; realescape/rebinding tests/G-04. |
| R06 replay/partial/unknownexternal result | Critical/reliability | durableintent/claims/fences/NO_REPLAY; crashbefore/after/partial/lease tests. |
| R07 ledger/grant/terminaloutcome race | Critical/ledger | atomiclocks/constraints/immutablekeys;100×20actor trials/exactreconciliation. |
| R08 DoS/amplification | High/operations | approvedinput/rate/gap/queuebounds; finitepools/retry/clocks; valid2×healthy and separatefault tests. |
| R09 backup/resurrection/remanence | Critical/privacyrecovery | separatecontent/syncmetadata/currentcontrols;G-03 erasure+restore/failover/no-oldcontext tests. |
| R10 supplychain/credentialcompromise | Critical/delivery | immutablelocks/fullSHAactions/SBOM/provenance/scans/rotationproof; noreleasedetectedsecret/knownexploitedrisk. |
| R11 audit/administratormisuse | Critical/securityreview | pre-processingrequiredevidence, attributedboundconfirm, independentwitness; tamper/purge/missingstore tests. |
| R12 observabilityfalsehealthy | High/operations | mandatoryproducers versus optionalexport; capabilityreadiness/independentalertdelivery states; all-sink lossFAIL. |
| R13 oldschema/startupmigration drift | Critical/migration | actualcatalogG-02; pairedAlembiconlyjob; clean/upgrades/queryconsistency tests; no conditionalmasking. |
| R14 mutable/host-coupled/ineffectiveDocker | High/delivery | reviewedimages/identitymanifest/projecttopology; effectiveUID/caps/limits/volumes/network inspection. |
| R15 incompletelegacyverification/oversizedmixedmodules | High/architectureQA | approvedledgertrace, redfirst/fullqualitygates, inwardimports/cycles/500logicalline review. |

Detailedstatus perrisk: OPEN until namedowner, evidence, remediation, verification
artifact and explicitreviewedclosure recorded. No acceptedexception exists.

## Required telemetry, availability and SLOs

Mandatory redacted correlation/admission/audit/businessoutcome producers remain durable.
Optional OTel/log/metricexporterdegradation alone permits only independentlysafework;
exportfilter isn't primaryredaction. No automaticHTTP/SQL capture ofsecretURL/text.
Ownedalertproduction<=300s, deliveryCONFIRMED/UNKNOWN/NOT_DELIVERED separately; all
evidencesinksdown cannot earncoverage. Independentobserverproposed15sprobes/2stimeout/
twofailuresconfirmation; immediateintegrity/security alerts. Actualhostloss observer
mustbe independentlyplaced. Liveness process/eventloop only; no upstreamrestartstorm.
Readiness percapability: schema/currentcontrols/syncdurability/evidence/egress valid;
AI unreadywithprovider, walletselfservice mayremain safe; historyunavailable doesn't
silently replace promisedstoredcontext.

ApprovedSLIs:>=99%eligibleinteractiveconfirmedprogress<=2s;>=95%answer/safeterminal<=20s;
everyaccepteddurableitem completion/safeauthorizedresumption/review<=300safterrestore;
0duplicatevisible/ledger effects;0lostacceptedwork;queueage120s;alertproduction300s;
exactledger/import;restore+rollback60min;release/rollback30mineach.
Unknownreviewisn't a confirmedanswer/safeterminal forSC003; SC011durablereferralis
a valid recoverydisposition, nothumanresolutiondeadline. Losttransienttext is explicit
safe/review disposition, never claimed replayablecontent. Healthyquestionscan'tpass
byfast errors; provider/faultdenominators predeclared. PassiveAdsnoackspam.

Additional PROPOSEDproduction SLO forownerreview: capability availability99.5%perrolling
30 days (~216minerrorbudget), measured5s independent probes ofready+successfuladmission
ofdeclaredvalidsynthetic non-privileged journey, not merelyTCP/liveness. Includemaintenance/
upstreamunavailability; no posthocexclusions; missingprobe/evidence countsunavailable.
Admission cannot be fabricated whenrequiredevidence isdown. This is not an approvedspec
promise/hostingguarantee; target unresolved untilownerplan approval. Capabilityerror/
providerhealth/Adsdelay/jobsuccess additionally reporttypedfailure andconfirmed/
unknown proportions; finitequeue/provider/grace deadlines asabove, no unknowncountPASS.

## Supply chain, regions and cost gates

Futureidentitymanifest: sourcecommit, reviewedplatform/Python/base/runtime/PG/gateway/
ops/spec images bydigest, completehash-verifiedtransitivewheellock/OSpackages,
CIactionsfullSHAs, license/SBOM/provenance/secret/dependency/static/vulnerability/malware
scans, securitysettings and frozenmeasurement inputs. Buildfromapprovedwheelhouse;
no undeclarednetwork orsourcehistory/secrets/dumps/logs/caches incontext. Provenance
doesn'tprovebenignsource. Critical/knownexploitedvulnblocksrelease absentexplicit
time-boundedconstitutionexception. ForkPRCIhasnosecrets/privilegedartifacttrust.
[pip hashes](https://pip.pypa.io/en/stable/topics/secure-installs/),
[GitHub CI security](https://docs.github.com/en/actions/reference/security/secure-use).

Capacity totals includeall dependencies; new paidresourceauthority0 atpresent.
Actualmonthlyall-incurrency/cap, independenthosting/storage/network/provider/
observer/witness/backup costs, supportedCPU/imageplatform, regions/residency/
subprocessors and volumegrowthbudgets NOT SET;G-05 blocksdependentplan approval.
No guessedprices/domains/regions, globaldaemoninstall or newpaidservice purchase.

## Future runbooks / explicit ownership

Release/schema incompatibility→trafficgate,noentrypointmigration,reviewexactpair.
Metadata/standby outage→stopaffectedadmission/dispatch, preserveunknowns, fencedrecovery.
Provider/egress/privacy incident→disabledependentcapability,noalternativeprovider.
Ledger/import mismatch→holdscope,independentexactreconcile,noinventedevents.
Contentclear/expiryfailure→denyreads/store, preservegenerations,erasureinvestigation.
UnknownTelegramresult→current SA/scopedoperatorread-onlyproof,NO_REPLAY / no automatic refund.
Secretincident→ownerseparaterotation/disableintegration,redactedreport,nevercopysecret.
Capacity/queue breach→pre-admitrefusal/safedisposition, no acceptedeviction/clockreset.
Eachrunbook needs namedowner+alternate, evidence/decision/abort/recovery checklists,
verifiedcommands and timeddrill artifacts after approval. No operational commands now.
