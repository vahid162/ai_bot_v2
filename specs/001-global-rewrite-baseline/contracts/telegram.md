# Telegram transport V1

DRAFT; no Telegram connection, bot setting change or code. Initial transport is
outbound-only long polling; no public webhook/FastAPI requirement. PTB Bot API is
an adapter, not the domain/application model.

## Reception and identity

One current DB-fenced ingress lease per botnamespace. Startup verifies schema/control/
durability and inspects webhook state only under future integration authority.
Unexpected webhook/conflictingreceiver→UNREADY; no automatic deleteWebhook,
drop_pending_updates, negativeoffset or ownership takeover.

Use application-owned get_updates loop: reserve bounded volatile capacity, validate
physical bounds, commit sanitized acceptance/dedupe/terminal dispositions+required
audit/cursor, then request next higher offset. Storefailure→nooffsetadvancement.
Default Updater RAM enqueue is not durable acceptance.
[Telegram getUpdates](https://core.telegram.org/bots/api#getupdates),
[PTB Updater](https://github.com/python-telegram-bot/python-telegram-bot/blob/v22.8/src/telegram/ext/_updater.py).

Idle>week may randomize updateIDs; persist verified idle/bootstrap state and probe
with omittedoffset without acknowledgement before validated cursorprogression.
Do not blindly discard a lower random ID as historical or retag it into a new epoch.
Production idle/reset protocol must pass contract tests before activation.
Telegram24h retention is not application dedupe or recovery.
Businessidentity=trustedsourceepoch+tenant+originalnamedauthor+originalmessage/media-groupID;
forward source age is irrelevant to new forwarded message creation; editdate never
refreshes original<=24h admission age. Duplicate/restore/restart preserve fences.

Raw updates/questions/captions/replies/provider answers remain bounded memory.
Durable envelope contains scope/identity/revision/route/noncontent evidence only.
Lost body beforedispatch→INPUT_UNAVAILABLE/noeffect safe disposition; afterdispatch
intent→UNKNOWNreview. Never re-fetch/re-generate old raw content to bypass privacy.

## Routing, actor and binding

One parser strips actual command prefix once, preserves body, rejects foreignbot
suffix/unknownmalformed commands without provider/mutation. Use exact approved
[route inventory](../recommended-contracts.md#verified-names-and-intentional-route-repairs);
repair only /ads add/notad. /ads balance|bal is example selection, not wallet;
/ads_probe is standalone. Retired usercleanup values cannot return destructive behavior.

All privateplain,/askargs,eligible replytext/caption and matching ForceReply use one
conversation application pipeline. Argswin; emptyquestionopensbound5minprompt,noAI.
Current groupOFF/address/all/adminonly applies every route. Staleadmin/prompt replies
never AI fallthrough; newcommands cancelaskprompt. No autoanswer for AI edits.
Anonymous exact-groupprincipal needs live platform evidence; reservednumericID/forward
cannot confer group/global/namedwallet/history/vote authority.

Callbacks carry opaque nonce<=64UTF8bytes and server-side original actor/tenant/topic/
action/target/prompt/answer/revision/expiry. Every page/read/write reauth/audit.
Changed selectedgroup never retargets pendingconfirmation. Panels15minidle/1habsolute;
confirmation5minabsolute. /users privatecurrent SA,max50ascendingID,minimalstatusonly.
DMdeny blocks ordinaryconversation only; independently authorizedselfservice/admin
is not silently revoked.

## Sending and ambiguity

Before each effect: current original actor/target/botpermissions, DMdeny where
applicable, mode/config/activation/consent/generations/deadline; durable DISPATCHING
attempt/currentfence then network. Valid authenticated exactmethod receipt establishes
APPLIED_CONFIRMED. Timeout/cancelafterstart/ambiguous5xx/malformedresponse/generic
PTBtimeout→UNKNOWN, no automatic retry. Queue/dedupe/providerresult is not proof.
No generic Telegram getMessage/resultlookup or exactly-once is promised.

Proven nonexecution only:<=3totalattempts/30s fromfirstattempt, waits1s then3s plus
boundedpositive0–20%jitter; currentgates/remaindeadline; requiredRetryAfter must fit,
otherwise safe terminal/review, never dispatch earlier. UNKNOWN stops immediately.
Transportresult and tokendebit remain separate; confirmedADwarning plus currentwarn
and unsetpaididentity alone can authorize potential1-unit atomiccharge.

Final renderedanswer<=16000UTF16units,<=6orderedchunks<=3000each includingnotice;
safeplainescaped output, completecharacter/entity boundaries. Eachchunk has stable
effect/receipt; onunknown preserveconfirmedchildren/stop, no wholeanswerreplay.
Feedback only finalconfirmedchunk. Exports owner-only/private; unknown/unavailable
private delivery does not publish transcript in group or falsely report success.
Manualmute has no hiddenmemberdeletion; botcleanup only knownbot-ownedmessages.

## Reference versus real limits

Product input/rate/gap/album/workbounds are approved D-012. Controlled200op/sfakebot
and10/sfakechat are test limits only. Proposed production throttles25messages/sbot,
1/sprivatechat,18/mingroup need actual method-specific quota evidence and owner
integration review; provider promises unaffected. Partialchunks and ratefaults test
separately, not hidden from healthyreference denominators.
[Telegram send guidance](https://core.telegram.org/bots/faq#my-bot-is-hitting-limits-how-do-i-avoid-this).
