# Proposed synthetic fixture and corpus seed

Status: Draft seed v0.1, 2026-10-11. All content below is invented; domains are reserved .example names, actors/tenants/messages are symbolic references, and there are no real Telegram accounts, secrets or copied conversations. No provider/Telegram call, corpus execution or deletion activation has occurred.

This seed has 30 text/caption items: 15 proposed AD, 10 proposed NON_AD, 5 REVIEW. Labels were drafted during specification work, NOT independently labelled, NOT project-owner approved and NOT evidence of safety. It is deliberately too small to claim representative coverage or statistical error bounds. REVIEW means unresolved gold label, not safe to silently omit, relabel or count as an approved negative. No item can satisfy D-005 until independent review resolves labels and the owner approves the versioned coverage.

## Required approval/evaluation record

Before specification approval, define owner-approved corpus coverage and label/adjudication rules, including per-language, non-exempt legitimate negatives, genuine eligible positives, contact-like content, neutral links, quotes/reports, long/short text, supported media/edits/forwards, albums and exemptions. Independent reviewers (not the rule/classifier author acting alone) label items without viewing candidate decisions; disagreements and REVIEW labels are adjudicated before gold freeze. Record reviewer identity/role, provenance, consent if any real sanitized material is later authorized, text/metadata checksum, proposed→gold changes, owner sign-off and corpus revision. Keep training examples separate from held-out evaluation; cannot validate on a corpus chosen only to match known classifier outputs.

Actual evaluation after authorized implementation must record frozen corpus/version, policy/configuration/threshold revision, eligible scope, expected actions, candidate classification and per-item/member effects, false proposed deletions, missed positives, exemptions and malformed/unknown outputs. Report all categories/locales, including blocked/unresolved items. Owner activation is a separate later record tied to the matching evaluation/scope/configuration; this document sets activation=false for all present work.

## Seed context and label/action separation

Text/photo-caption fixtures validate text/caption policy only; they do not prove visual image/video-content classification. Mixed-content albums require independent logical and member-level disposition labels; an ad caption must not conceal a destructive proposal for a legitimate member.

Default illustrative context: tenant T-A, regular actor U-A, topic TOP-A, global/group AdsGuard enabled, no exemption, one logical item, wallet balance 1, mode delete. This is a hypothetical evaluation setup only: real automatic deletion remains disabled. Proposed NON_AD implies no destructive action in candidate evaluation; approved eligible AD positives must produce the D-007-approved expected policy action, not evade the gate by always returning no action. AD is a semantic label, not permission to act: exemptions, mode, uncertainty and D-005 activation remain separate. REVIEW has no approved expected decision. All bounds, rates, timeouts and grace values require an approved contract, not a copied legacy default.

| Item | Locale | Content kind | Proposed label | Literal synthetic content |
| --- | --- | --- | --- | --- |
| COR-FA-N1 | fa | text | NON_AD (unapproved) | چطور تنظیمات زبان گروه را تغییر بدهم؟ |
| COR-FA-N2 | fa | text | NON_AD (unapproved) | راهنمای پروژه در https://docs.example/guide است؛ برای حل خطا کدام بخش را بخوانم؟ |
| COR-FA-R1 | fa | text | REVIEW (unapproved) | این پیام «همین امروز خرید کنید» را برای بررسی تبلیغ گزارش می‌کنم، نه برای فروش. |
| COR-FA-A1 | fa | text | AD (unapproved) | فروش ویژهٔ دورهٔ آموزشی؛ همین امروز ثبت‌نام کنید و تخفیف بگیرید. |
| COR-FA-A2 | fa | text | AD (unapproved) | کفش با تخفیف می‌فروشیم؛ خرید از https://shop.example/sale |
| COR-FA-A3 | fa | photo_caption | AD (unapproved) | برای سفارش این محصول به https://shop.example/order بروید. |
| COR-EN-N1 | en | text | NON_AD (unapproved) | How can I change this group's language setting? |
| COR-EN-N2 | en | text | NON_AD (unapproved) | The project guide is at https://docs.example/guide. Which section explains this error? |
| COR-EN-R1 | en | text | REVIEW (unapproved) | I am quoting 'Buy today' to report a possible ad, not to sell anything. |
| COR-EN-A1 | en | text | AD (unapproved) | Special offer on our training course. Enroll today for a discount. |
| COR-EN-A2 | en | text | AD (unapproved) | Discount shoes for sale. Buy at https://shop.example/sale |
| COR-EN-A3 | en | photo_caption | AD (unapproved) | Order this product at https://shop.example/order. |
| COR-AR-N1 | ar | text | NON_AD (unapproved) | كيف أغيّر إعداد لغة هذه المجموعة؟ |
| COR-AR-N2 | ar | text | NON_AD (unapproved) | دليل المشروع في https://docs.example/guide. أي قسم يشرح هذا الخطأ؟ |
| COR-AR-R1 | ar | text | REVIEW (unapproved) | أقتبس عبارة «اشتر اليوم» للإبلاغ عن إعلان محتمل، وليس لبيع شيء. |
| COR-AR-A1 | ar | text | AD (unapproved) | عرض خاص على دورتنا التعليمية؛ سجّل اليوم واحصل على خصم. |
| COR-AR-A2 | ar | text | AD (unapproved) | أحذية مخفّضة للبيع؛ اشتر من https://shop.example/sale |
| COR-AR-A3 | ar | photo_caption | AD (unapproved) | اطلب هذا المنتج من https://shop.example/order. |
| COR-TR-N1 | tr | text | NON_AD (unapproved) | Bu grubun dil ayarını nasıl değiştirebilirim? |
| COR-TR-N2 | tr | text | NON_AD (unapproved) | Proje kılavuzu https://docs.example/guide adresinde. Bu hatayı hangi bölüm açıklıyor? |
| COR-TR-R1 | tr | text | REVIEW (unapproved) | Bir şeyi satmak için değil, olası bir reklamı bildirmek için 'Bugün satın alın' sözünü alıntılıyorum. |
| COR-TR-A1 | tr | text | AD (unapproved) | Eğitim kursumuzda özel fırsat! Bugün kaydolun ve indirim kazanın. |
| COR-TR-A2 | tr | text | AD (unapproved) | İndirimli ayakkabı satıyoruz. https://shop.example/sale adresinden satın alın. |
| COR-TR-A3 | tr | photo_caption | AD (unapproved) | Bu ürünü https://shop.example/order adresinden sipariş edin. |
| COR-RU-N1 | ru | text | NON_AD (unapproved) | Как изменить настройку языка этой группы? |
| COR-RU-N2 | ru | text | NON_AD (unapproved) | Руководство проекта находится на https://docs.example/guide. В каком разделе описана эта ошибка? |
| COR-RU-R1 | ru | text | REVIEW (unapproved) | Я цитирую «Купите сегодня», чтобы сообщить о возможной рекламе, а не чтобы что-либо продать. |
| COR-RU-A1 | ru | text | AD (unapproved) | Специальное предложение на наш учебный курс! Запишитесь сегодня со скидкой. |
| COR-RU-A2 | ru | text | AD (unapproved) | Обувь со скидкой в продаже. Покупка на https://shop.example/sale |
| COR-RU-A3 | ru | photo_caption | AD (unapproved) | Закажите этот товар на https://shop.example/order. |

## Structural variants (non-content fixture definitions)

Each variant requires the approved source item label and an independent eligibility/action oracle. Variant IDs are not additional independently labelled content samples. Cross products are reported by declared manifest, not selectively sampled after results.

| Variant | Synthetic setup | Expected constraint / pending oracle |
| --- | --- | --- |
| STR-01 | For each locale's A1, change only actor to current/exempt administrator, then allowed user. | Semantic AD unchanged; approved exemptions prevent destruction. Revoked roles must not stay exempt. AC-BR010-01, AC-BR011-04. |
| STR-02 | For each A2 and N2, vary approved allowlisted domain vs unlisted, subdomain, lookalike and malformed URL in T-A and T-B. | Exemption scope and normalized-domain contract D-007; no other-tenant allowlist effect. AC-BR010-01, AC-BR013-03. |
| STR-03 | Synthetic ordinary address/reply, forwarded origin and automatic discussion-forward metadata, preserving text. | Address/reply/forward eligibility still awaits D-007/D-010; D-007's automatic guard mode rule is settled: off/none/warn cannot delete user content, delete remains conditional on all gates. Never assume legacy pre-mode deletion or address shortcuts are approved. AC-BR003-02, AC-BR010-03. |
| STR-04 | Captionless photo/video, late caption, edit before/at/after grace, caption reply and mode off/none/warn/delete. | Approved D-007 mode oracle: off/none have no moderation action, warn never deletes user content, delete only permits deletion with eligibility/authorization/exemptions and D-005 gates. Profile combinations cannot bypass mode; exact caption/grace/cancel/notice oracle remains pending. No present activation approval. AC-BR011-02/03. |
| STR-05 | Three-member album AL-A: M-A1,M-A2,M-A3; A3 caption on first or last member; duplicate/reordered delivery. | One logical moderation/spend identity; per-member known/unknown outcomes. No IDs from a real chat. AC-BR010-04, AC-BR012-02. |
| STR-06 | Classifier output null, malformed type, missing score, untrusted output, timeout, or score just below/equal/above approved threshold. | Invalid/unknown evidence cannot authorize destruction; valid boundary comparison awaits D-007. AC-BR010-02. |
| STR-07 | Trusted eligible positive × modes × wallet 0/1 × global/group token switches × single/album × duplicate. | D-002 exhausted-wallet contract; approved D-008 no-action branch gives zero AdsGuard spend/debit or successful-spend event with wallet 0/1, any token switches, single/album, duplicate/edit and queued/recovered evaluation. Current mode before uncommitted debit blocks stale charging; no automatic historical refund and no retro-charge of recorded none outcome. Independent authorized grants are separate. Warning/deletion consuming branches/numbers remain pending. No insufficient successful debit. AC-BR011-01, AC-BR015-01/02, AC-BR012-04. |
| STR-08 | Quality record absent/failed/revoked/out-of-scope; activation absent or scope/config changed; new/queued/recovered delete candidate; forward/caption work scheduled in delete then changed to off/none/warn or profile changed. | Zero automatic deletion attempts in each invalid-gate or non-delete-mode case. A matching valid gate never bypasses current effective mode or D-003 unknown holds. Valid delete-mode/gate scenario is future simulated test evidence, never present activation permission. AC-BR011-02/05, AC-BR012-04. |
| STR-09 | Send disconnect: confirmed completion, trustworthy confirmed non-execution or no sufficient evidence; partial album mixture; restart and duplicate delivery. | D-003 hold/no automatic repeat for unknowns; no refund or external exactly-once assumption. AC-BR002-04, AC-BR012-01/02/03. |
| STR-10 | Zero-balance spend, duplicate before/after a new grant, old replay after cleanup/restore, and equal keys across two tenants/users. | Preserve actual insufficient outcome and no second charge; D-008 final replay/correction horizon pending. AC-BR015-01–05. |
| STR-11 | Forged callback/prompt/role, group A→B switch, stale admin, another owner's export/report ID and private-language callback from group; explicit private denial × all eight global/environment/managed allowance combinations; queued private dispatch, restart, ordinary allow/global toggle, authorized/unauthorized explicit unblock and missing historical allow row. | No cross-scope read/write. D-006: explicit denial always wins private conversation, only current superadmin explicit unblock clears it, unblock still grants no allowance or feature bypass. Unknown prior effects follow D-003; missing legacy allow is not inferred block. No group moderation/role/history erasure/Telegram platform blocking. AC-BR004-01, AC-BR005-03, AC-BR006-01–03, AC-BR007-01–04, AC-BR009-02. |
| STR-12 | Synthetic marker PRIVATE_MARKER_A and fake credential CREDENTIAL_MARKER_A in failures/import inputs/catalog variables. | Markers never leak to unauthorized health/log/export/diagnostic paths; no real credential included. AC-BR001-04, AC-BR018-01/02, AC-BR021-01. |

## History and import boundary fixtures

- HIS-01: U-A/T-A has never opted in. A new question/answer at 2026-10-01T12:00:00Z may be processed as a current exchange but creates zero durable history text or context-bearing retained copy. Later opt-in cannot backfill it.
- HIS-02: An explicitly consenting owner's question created 2026-10-01T12:00:00Z expires 2026-10-31T12:00:00Z; a distinct answer created 12:00:02Z expires two seconds later. Test just before, exactly at and after each boundary. Reads/edits/export/replay do not reset either clock; expiry makes text inaccessible at the boundary and deletion automatic. Physical scheduling tolerance is not permission to expose expired text.
- HIS-03: Clear consenting history on 2026-10-10T09:00:00Z, restore an older synthetic backup, then read/export/AI-context/feedback. Cleared text must not resurrect; separately governed ledger/audit remains. Future opt-out without clear is a distinct fixture.
- HIS-04: A synthetic legacy batch contains old Q&A, provider/session references, users/settings, wallet/grant/ledger/dedupe rows, ambiguous roles/tenant IDs, a credential marker and unknown pending deletion. D-011 selects validated wallet/ledger continuity only after separately approved actual mappings and exact tenant/user reconciliation, with minimum validated linked grant/dedupe evidence to prevent a second grant/debit. Repeat the hypothetical import and lazy/scheduled/replay checks; row existence cannot turn a failed spend into success. Missing/conflicting evidence is rejected/quarantined and blocks promotion/cutover; no fabricated balance/event or historical refund. Exclude all legacy history/context/secrets, preserve unknown-effect holds and count every disposition. Other entity imports/supporting identities/lifecycles remain separately approved. This fixture is NOT RUN; no source data deleted or real migration run.

## Coverage still needed before corpus approval

Owner-approved held-out population/label counts, representative message-length and media mix, contact-like and adversarial semantics, multilingual independent review, exemptions and topic/album variants, threshold/configuration scope, supported edits/forward origins, production-like sanitized additions if separately authorized and complete manifest of all cases. The 30-item seed is a starting point, not a completed approved acceptance corpus. Measurement-contract.md defines how future results are counted; D-012's Standard/counting/sample/test-window subset is selected via owner delegation, not approval of this corpus, coverage, independent labels or actual results. Actual tests and activation are later approval gates.
