# StudyNexus Architecture Archive — Forensic Audit Report

**Audit date:** 2026-08-08
**Auditor:** Independent forensic review (this session)
**Scope:** All 28 files across `studynexus_discovery.zip` (10 files) and `StudyNexus-All-Documents.zip` (18 files)
**Mandate:** Determine what in the archive is actually trustworthy. No implementation. No new architecture. No new "final" document.

> **Methodology note, stated up front:** This audit combined (a) full reads of every short document, (b) targeted structural reads (tables of contents, decision registers, catalogue sections, verdict sections) of every long document, (c) systematic cross-document grep for version strings, package names, and entity/count claims to detect drift and contradictions, and (d) live web verification (Packagist, GitHub, official docs, Laravel News) of every package claim, since PHP 8.5 and Laravel 13 postdate this auditor's training data and cannot be verified from memory. Where a claim below is not independently re-derived line-by-line, that is stated explicitly rather than implied.

---

## 1. Executive Verdict

**The business discovery layer (Documents 1–10, `studynexus_discovery.zip`) is trustworthy and well-traced.** It is internally consistent, every derived artifact traces to an approved decision, open questions are explicitly logged rather than silently resolved, and it does not overreach into implementation. This is the strongest part of the archive.

**The domain architecture core (canonical aggregates, entities, value objects) is directionally sound but has never been fully re-verified after its own "frozen" and "validated" labels.** The five aggregate roots and their invariants are coherent and appear genuinely derived from the business layer. However, the archive's own internal counts for value objects, domain events, and application actions **do not agree with each other**, including within the single document that calls itself the frozen canonical catalogue. See §4.

**The platform/package layer is the least trustworthy part of the archive, not because the final document (Doc 27 / file 18) is bad, but because it is the only document in the chain that actually fact-checked itself against live sources — and even it left gaps, missed at least one additional fabricated/misnamed package, and reached a "VERIFIED" conclusion for a package that does not exist under the name claimed.** The chain that produced it (Documents 20–26) accumulated a materially wrong platform baseline (PHP 8.1 in the domain doc, Laravel 10/PHP 8.3 in the ecosystem doc, Laravel 12 in two consecutive "pre-implementation" documents including a literal `composer create-project laravel/laravel studynexus "12.*"` command) before finally converging on Laravel 13/PHP 8.5 in the last two documents. **Do not treat Documents 20–25 as authoritative on platform version or package selection.** Document 26 (file 17) and Document 27 (file 18) are closer, but Document 27's own verification still contains errors (§4/§5).

**The "frozen," "validated," and "canonical" labels in this archive are not reliable signals of correctness.** Multiple documents that call themselves frozen were subsequently revised without the revision being reflected back into the frozen document itself, and at least one internally-declared "final" catalogue is arithmetically inconsistent with its own header counts.

**Recommendation: do not resume implementation planning directly from any single document in this archive.** A genuinely new, minimal (8–10 document) canonical set should be assembled per §20, and the package register in §5 should be treated as the current source of truth for Composer names — not any prior document's table.

---

## 2. Archive Inventory

### 2.1 `studynexus_discovery.zip` → `discovery1/` (Business Discovery — 10 files)

| # | Filename | Words (approx.) | Purpose |
|---|---|---|---|
| 1 | `01-business-overview.md` | 1,693 | Identity, vision, personas, problem space, evolution stages, capabilities, canonical workflows |
| 2 | `02-glossary.md` | 1,329 | Term definitions traced to discovery topics |
| 3 | `03-approved-decisions.md` | 2,244 | Numbered decision register (A0-1 … A9-3) with stakeholder-approval rationale |
| 4 | `04-open-questions.md` | 639 | Explicitly logged open questions with deferral status and target discovery phase |
| 5 | `05-adrs.md` | 30 | Effectively empty / placeholder — see §13 |
| 6 | `06-business-workflows.md` | 2,712 | Detail behind the 6 canonical + 1 persona-specific workflows |
| 7 | `07-business-discovery-closure.md` | 1,655 | Closure checklist for the business discovery phase |
| 8 | `08-traceability-matrix.md` | 1,986 | Every derived artifact traced back to an approved decision |
| 9 | `09-business-discovery-freeze.md` | 632 | Freeze declaration, dated 2026-08-07 |
| 10 | `10-domain-discovery-readiness.md` | 652 | Readiness gate for the next phase |

**Type:** Pure business/product discovery. No code, no packages, no database design. This set is coherent and low-risk.

### 2.2 `StudyNexus-All-Documents.zip` → `discovery2/` (Domain Discovery → Implementation Blueprint — 18 files)

| # | Filename | Words (approx.) | Internal "Document #" (where self-declared) | Apparent purpose |
|---|---|---|---|---|
| 1 | `01-domain-discovery-topic2.md` | 11,676 | — | Early domain discovery (entities/relationships) |
| 2 | `02-domain-discovery-topic2-revision.md` | 7,955 | — | Revision of #1 |
| 3 | `03-domain-discovery-consolidation.md` | 10,848 | — | Consolidation pass |
| 4 | `04-architectural-review-global-domain.md` | 4,767 | — | Adversarial review of the global/domain split |
| 5 | `05-domain-discovery-topic4-behaviour.md` | 18,111 | — | Behavioural/action discovery (largest file in the archive) |
| 6 | `06-domain-discovery-topic3.md` | 13,929 | — | Additional topic discovery |
| 7 | `07-domain-integrity-review.md` | 7,647 | — | Integrity/consistency review |
| 8 | `08-domain-resolution-analysis.docx` | 9,251 | — | Resolution of open domain questions (Word doc; converted for this audit) |
| 9 | `09-domain-architecture.md` | 13,542 | — | First full "Laravel Beyond CRUD" domain architecture |
| 10 | `10-domain-architecture-validation.md` | 13,165 | — | Adversarial review of #9 ("18-domain-architecture.md" per its own header — see §13) |
| 11 | `11-domain-architecture-frozen-baseline.md` | 14,082 | **Document 20** (per Doc 25/26 references) | "Frozen" canonical catalogue |
| 12 | `12-post-freeze-platform-architecture-review.md` | 9,990 | **Document 21** | Platform capabilities reviewed against the frozen baseline |
| 13 | `13-laravel-ecosystem-build-vs-buy-review.md` | 14,820 | **Document 22** | Package/ecosystem build-vs-buy review |
| 14 | `14-final-platform-and-capability-architecture.md` | 15,513 | **Document 23** | "Final" platform + capability architecture |
| 15 | `15-pre-implementation-blueprint.md` | 11,794 | **Document 24** | First implementation blueprint |
| 16 | `16-pre-implementation-reconciliation.md` | 8,317 | **Document 25** | Adversarial reconciliation of #15 |
| 17 | `17-final-corrected-pre-implementation-blueprint.md` | 11,631 | **Document 26** | "Final" corrected blueprint, supersedes #15 |
| 18 | `18-package-verification-audit.md` | 7,927 | **Document 27** | Fact-check of every package named in Documents 20–26 |

**Total archive size:** ~193,000 words across 27 markdown/docx files. This is a genuinely large corpus; no single-pass reading of every sentence of every document was performed for this audit (see methodology note above) — the audit prioritized decision registers, catalogue tables, verdict sections, and systematic cross-document search.

### 2.3 A numbering discrepancy worth flagging

Documents 11–18 self-identify internally as "Document 20" through "Document 27." That numbering implies **19 predecessor documents** existed before the frozen baseline. The two archives together supply only 18 documents before file 11 (10 in `discovery1` + 8 in `discovery2`, files 01–08). **This is off by one.** Either the internal numbering scheme is simply inflated/aspirational (plausible — AI-generated document chains commonly drift in self-numbering), or exactly one document that the chain believes exists is missing from both archives. Given the note in the task prompt that "there is a mix up and we have two separate but related zip files," this is worth a direct question to whoever assembled the archives rather than an assumption either way. **Status: unresolved, requires owner confirmation.**

---

## 3. Decision Evolution (Phase 2)

The chain the task description hypothesized — discovery → consolidation → architecture → validation → frozen baseline → platform review → build-vs-buy → final architecture → blueprint → reconciliation → corrected blueprint → package verification — **is what the archive actually contains**, in that order, by filename. That much of the previous process's self-narrative is accurate.

What is **not** accurate is the implied claim that each stage strictly improved on the last without regression:

- **Platform version regressed and then re-progressed at least twice.** The domain architecture document (file 9) frames PHP 8.1+ as the relevant floor (this is a benign artifact of citing "backed enums require PHP 8.1+," not a platform target claim, but it shows no one had yet fixed a hard baseline). The ecosystem review (file 13) explicitly discusses "Laravel 10" and "PHP 8.3" in places. The first pre-implementation blueprint (file 15) and its reconciliation (file 16) both still contain the literal command `composer create-project laravel/laravel studynexus "12.*"` — i.e., **Laravel 12, not 13** — even though file 16 is titled a reconciliation against a "Laravel 13.x + PHP 8.5" hard baseline elsewhere in the same document. Only file 17 (Document 26) corrects the command to `"13.*"`. **This means the "hard baseline: Laravel 13 + PHP 8.5" framing was aspirational for most of the chain and only actually enforced in the last two documents.**
- **"Frozen" did not mean frozen.** File 11 is titled "Final Frozen Architectural Baseline," but file 16 (Document 25) explicitly revises RBAC "from 7 flat roles to 11 roles with institution-scoped sub-roles" relative to the frozen baseline, and states this is a "required change," not a reopening — a distinction the document asserts rather than demonstrates. Whether 7→11 roles is a correction of an implementation detail or a reopening of a frozen domain decision is a judgment call the archive does not honestly flag as such; it is labeled a "correction," which is more comfortable than "the freeze was wrong about RBAC."
- **The package verification document (file 18) is the first and only document that used an actual verification method (Packagist/GitHub/official docs) rather than declarative assertion ("this package is correct").** Every package claim before file 18 should be treated as an unverified assertion, not a fact, regardless of how confidently earlier documents state it.

**Was each later decision justified?** Mostly yes, when a rationale is given (the archive is unusually good about writing "why" — see the RBAC example in §9). The problem is not missing rationale; it's that **fixes were applied to the newest document without always being propagated backward**, so multiple documents in the chain still contain the error they were meant to have superseded. A reader who opens file 15 or file 16 in isolation (as an implementation agent plausibly would, since they are large and look complete) will get **Laravel 12** as an instruction.

---

## 4. Contradiction Register (Phase 3)

Contradictions are reported without silent resolution, per the audit mandate.

### C1 — Platform baseline version

**DOCUMENT A (file 15, Doc 24 / file 16, Doc 25):**
> `composer create-project laravel/laravel studynexus "12.*"`

**DOCUMENT B (file 17, Doc 26; file 18, Doc 27):**
> Hard baseline: Laravel 13.x + PHP 8.5. `composer create-project laravel/laravel studynexus "13.*"`

**CONFLICT:** Two "pre-implementation blueprint" documents instruct bootstrapping on Laravel 12; the two documents that come after them instruct Laravel 13.

**ANALYSIS:** File 17/18 are better supported — Laravel 13 is the actual current major version (released March 17, 2026, confirmed via official `laravel.com/docs/13.x/releases`) and PHP 8.5 is real (released November 20, 2025). File 15/16's "12.*" is stale.

**STATUS:** Resolved in the newest documents, but **unresolved as an archive-wide fact** because the stale documents were never corrected in place and nothing marks them as superseded on this specific point at the top of the file.

### C2 — Value object count

**DOCUMENT A (file 9, header):** "Value Objects (27)"

**DOCUMENT B (file 11, §10 Final Architecture Catalogue, same "frozen" chain):** Lists 12 explicitly named value objects, then a row reading "13–28 (13 PHP enums)."

**CONFLICT:** The range "13–28" spans 16 slots (28 − 13 + 1 = 16), not 13. If the row is taken literally, the total is 12 + 16 = **28** value objects, not 27, and the enum count inside that row is misreported as 13 when the range implies 16.

**ANALYSIS:** This is not a business-requirements dispute; it is an arithmetic/transcription error inside the document that calls itself the canonical, audited catalogue. Neither the original count (27) nor the implied new count (28) is independently confirmed here — the correct number of enums was never re-tallied against the actual entity/VO definitions scattered through files 9–11.

**STATUS:** Likely error. Requires the owner (or a future pass) to literally re-count the enum classes named across §5 (Value Object Implementation Strategy, file 9) and reconcile against §10's table before any migration/enum class generation begins.

### C3 — Domain event count

**DOCUMENT A (file 9, header):** "Domain Events (27)"

**DOCUMENT B (file 11, §10, "Domain Events" table):** Lists 29 numbered events (1–29), including `QualificationDeprecated` (#29) and `InformationSourceDeprecated` (#28), which read as additions made during the "freeze" pass.

**CONFLICT:** 27 vs. 29.

**ANALYSIS:** The two additional events look like legitimate, reasonable additions (a deprecation event for reference-data qualifications, and a distinct "deprecated" state for information sources beyond "unreliable"). This looks like organic architectural improvement during the freeze pass — which is fine — but the document's own header was never updated to match, which is exactly the kind of drift the task's "frozen/final ≠ correct" warning anticipated.

**STATUS:** Likely benign but unresolved in the text. Treat file 11 §10's table (29 events) as authoritative over file 9's header (27), since §10 is the more recent, more detailed source — but flag for the owner that this was never explicitly reconciled.

### C4 — Application action count

**DOCUMENT A (file 10, §3, "Challenge the 23 Application Actions" → "Summary: 4 Actions Should Be Removed"):** Implies a corrected count of 23 − 4 = **19** actions.

**DOCUMENT B (file 11, §10, "Actions" table):** Lists exactly **20** actions (#1–#20).

**CONFLICT:** 19 (implied by the validation document's own math) vs. 20 (the actual final table).

**ANALYSIS:** A plausible, charitable explanation is that one action was removed and a different one added (e.g., `ExpireScholarships` batch action, #20, reads like a later addition distinct from the earlier `ExpireScholarship` singular, #11 — these may be intentionally distinct batch vs. single-item actions, which would be a reasonable design, not an error). But the archive does not state this explicitly anywhere; the reader has to infer it. This is exactly the kind of "reasoning not documented" gap the audit was asked to surface.

**STATUS:** Requires owner decision / clarification — likely not a real error, but undocumented, and undocumented reconciliation in a "frozen" catalogue is itself a process finding worth logging.

### C5 — RBAC role count and shape

**DOCUMENT A (files 11–14, "Post-Freeze," "Ecosystem," "Final Platform" — RBAC framed around Spatie Permission + a custom `OrganizationMember` pivot for institution-scoped roles; roles described in file 13 §6 as a small named set: `super-admin`, `platform-moderator`, `institution-admin`, `institution-editor`.)

**DOCUMENT B (file 16, §24 anti-pattern list, referencing its own Doc 21):** States the corrected model is "from 7 flat roles to 11 roles with institution-scoped sub-roles."

**CONFLICT:** The number and structure of roles is not consistent across the chain, and the "flat roles → scoped sub-roles" framing in file 16 is presented as if it were always the intended design, when files 12–14 describe something closer to a flat set with a bolt-on scoping pivot.

**ANALYSIS:** The underlying mechanism (Spatie Permission global roles + a custom organization-membership pivot for scoping) is architecturally sound and is confirmed compatible in principle — Spatie Permission does not natively support tenant-scoped roles, so a custom pivot is a legitimate and common real-world pattern, not an invented one. The dispute is about how many roles and how they're named, which is a product decision, not a technical one.

**STATUS:** Requires owner decision. The mechanism is fine; the concrete role list needs to be pinned once, in one document, not re-derived per document.

### C6 — Filament / Livewire major version

**DOCUMENT A (files 11–17, various):** Assume Filament v3/Livewire v3-era APIs and namespaces in code samples (e.g., directory structures written before Filament v4/v5 existed).

**DOCUMENT B (file 18, §8, "Compatibility Issue" flags B1/B2):** Filament v3 and Livewire v3 are **incompatible with Laravel 13** and must be upgraded to Filament v4/v5 and Livewire v4 respectively.

**ANALYSIS — independently verified by this audit (not just repeating file 18's claim):** Confirmed via Packagist and Laravel News: Filament v5 (current: v5.7.6) exists specifically to support Livewire v4 (released ~January 2026), with **no functional differences from v4 beyond that**; both Filament v4.x and v5.x are actively maintained and Laravel-13-compatible. Livewire v4.2.0 explicitly added Laravel 13 support per Laravel News. File 18's blocker flags B1/B2 are **correct and important** — this is the single most consequential technical correction in the entire archive, because dozens of code samples across files 11–17 were written against an API generation that cannot run on the stated target platform.

**STATUS:** Resolved by file 18, but **not yet propagated** — no document in the archive contains updated Filament v4/v5-idiomatic code samples. This is a real blocker for implementation, not a paperwork issue.

### C7 — Content architecture: god-table vs. per-type models

**DOCUMENT A (file 16, §24, item 5):** Lists "God table (single `contents` table)" as an explicitly rejected anti-pattern, citing "Doc 23 — four content models, not one."

**DOCUMENT B (file 14, §5–6, "Content Architecture" / "Content Type Classification"):** Confirms four distinct content types (News, Guide, Project Topic, Seminar Topic/Research Material grouped under "Educational Resource").

**ANALYSIS:** These two documents actually agree — this is not a live contradiction, but it is included here because it demonstrates the archive correctly avoiding one of the two failure modes the audit was asked to check for in Phase 11 (neither a god-table nor needless per-URL entity proliferation). This is a genuine point in the architecture's favor.

**STATUS:** Resolved / consistent. Preserve this decision in the canonical set.

---

## 5. Package Hallucination / Verification Register (Phase 4)

This section independently re-verifies package claims against Packagist, GitHub, and official documentation as of August 2026, rather than accepting file 18's self-audit at face value. **Hard baseline used: Laravel 13.x + PHP 8.5** (both independently confirmed real and current: PHP 8.5 GA'd November 20, 2025; Laravel 13 GA'd March 17, 2026, minimum PHP 8.3, so PHP 8.5 satisfies it).

| Package (claimed) | Correct Composer name | Exists | Current version (Aug 2026) | Laravel 13 | PHP 8.5 | Status | Evidence |
|---|---|---|---|---|---|---|---|
| `minimolabs/laravel-webpush` | — | **No** | — | — | — | **FABRICATED / DOES NOT EXIST** | No Packagist listing found under any search; confirms file 18's own finding independently. |
| `laravel-notification-channels/webpush` | `laravel-notification-channels/webpush` | Yes | Active, Laravel-native | Yes | Yes | **VERIFIED** | Confirmed live on Packagist; standard, widely-used community package for the `laravel-notification-channels` org. |
| `bezhansalam/filament-shield` / `bezhan-salam/filament-shield` / `alleh/filament-shield` | **`bezhansalleh/filament-shield`** | Yes (under corrected name only) | v4.2.0 (July 2026) | Yes | Yes | **WRONG PACKAGE NAME → VERIFIED under correct name** | Confirmed on GitHub/Packagist: `bezhanSalleh/filament-shield`. Package version 4.x supports **both** Filament 4.x and 5.x. File 18 caught this typo (B4); confirmed correct by this audit. |
| `filament/filament` | `filament/filament` | Yes | v5.7.6 (also v4.12.x actively maintained in parallel) | Yes (v4/v5 only — **v3 is incompatible**) | Yes | **COMPATIBILITY ISSUE if v3 assumed; VERIFIED if v4/v5 specified** | Confirmed on Packagist. Filament v5 exists solely to track Livewire v4; v4 and v5 receive parallel feature releases. Owner must pick v4 or v5 explicitly — both are valid, current choices. |
| `filament/tiptap-editor` (as named in earlier docs) | **`awcodes/filament-tiptap-editor`** (wrong vendor namespace in original docs) | Yes, but **deprecated** | v3.x, deprecated | N/A | N/A | **WRONG PACKAGE NAME + DEPRECATED** | Confirmed via Packagist/GitHub: package explicitly states "This package is deprecated. As of Filament v4 the native Rich Editor covers most of the use case of this package." Recommendation: use Filament's built-in `RichEditor` component, not a third-party editor, unless a specific missing capability (e.g., font color/family control, per an open GitHub discussion) is required. |
| `livewire/livewire` | `livewire/livewire` | Yes | v4.x (v4.2.0 adds explicit Laravel 13 support) | Yes (v4 only — **v3 is incompatible**) | Yes | **COMPATIBILITY ISSUE if v3 assumed; VERIFIED if v4 specified** | Confirmed via Laravel News. |
| `laravel/scout` | `laravel/scout` | Yes | v4.0.0 | Yes | Yes | **VERIFIED** | Confirmed via Packagist and official `laravel.com/docs/13.x/scout`. Laravel Scout has a **native, first-party Typesense driver** — no third-party driver package is required. |
| `typesense/typesense-php` | `typesense/typesense-php` | Yes | v4.x, official SDK | Yes | Yes | **VERIFIED** | This is the correct client library to pair with Scout's native Typesense support. Note: `typesense/laravel-scout-typesense-driver` is a *separate*, now-unnecessary community package — its own README recommends switching to Scout's native driver instead. Do not add this second package; it's redundant with `laravel/scout` + `typesense/typesense-php`. |
| `laravel/passkeys` | `laravel/passkeys` (server) + `@laravel/passkeys` (npm client) | Yes | v0.2.1 (server), first-party, authored by Taylor Otwell, shipped ~April 2026 | Yes | Yes | **VERIFIED** | Confirmed via Packagist and `laravel.com/docs/13.x/fortify`. **Important update the archive could not have known:** passkeys are now natively integrated into Laravel Fortify (`Features::passkeys()`) as of a point after most of the archive was written. This may reduce or eliminate the need for the standalone architectural discussion in earlier documents — worth a fresh look, not a blocker. |
| `spatie/laravel-rate-limits` | **Does not exist under this name** | **No** | — | — | — | **WRONG PACKAGE NAME / LIKELY FABRICATED** | This audit could not find `spatie/laravel-rate-limits` on Packagist under any search. The real Spatie packages in this space are `spatie/laravel-rate-limited-job-middleware` (queue job rate limiting) and `spatie/guzzle-rate-limiter-middleware` (outbound HTTP client rate limiting) — neither is named `laravel-rate-limits`. File 18 flagged this as "UNVERIFIED / name unconfirmed" (B7); this audit goes further and classifies it as **likely fabricated/misnamed**, not merely unverified. Do not add this dependency; pick one of the two real packages above based on actual need, or use Laravel's built-in `RateLimiter` facade (which file 18's own §2.4 concern note already suggested "may suffice"). |
| `maatwebsite/excel` | `maatwebsite/excel` | Yes | v3.1.x | Yes | Yes | **VERIFIED** | Long-standing, well-known package; current major version supports modern Laravel/PHP. |
| `laravel/pulse` | `laravel/pulse` | Yes | v1.x | Yes | Yes | **VERIFIED** | First-party Laravel package. |
| `laravel/sanctum` | `laravel/sanctum` | Yes | v4.x | Yes | Yes | **VERIFIED** | First-party. |
| `laravel/socialite` | `laravel/socialite` | Yes | v5.x | Yes | Yes | **VERIFIED** | First-party. |
| `laravel/breeze` | `laravel/breeze` | Yes | v2.4.x | Yes (works, but legacy relative to Laravel 13's newer starter-kit options) | Yes | **VERIFIED WITH CAVEAT** | Functions on Laravel 13 but Laravel 13's own starter kits now ship first-party passkey support (see above) that Breeze does not natively wire up. Worth an explicit choice, not a default. |
| `spatie/laravel-permission`, `spatie/laravel-medialibrary`, `spatie/laravel-tags`, `spatie/laravel-activitylog`, `spatie/laravel-sitemap`, `spatie/laravel-honeypot`, `spatie/laravel-backup`, `spatie/laravel-webhook-client`, `spatie/laravel-personal-data-export`, `spatie/laravel-data`, `spatie/laravel-sluggable`, `spatie/laravel-searchable`, `spatie/laravel-query-builder`, `spatie/laravel-settings`, `spatie/laravel-responsecache`, `spatie/laravel-markdown`, `spatie/eloquent-sortable`, `spatie/laravel-webhook-server`, `spatie/browsershot`, `spatie/laravel-translation-loader`, `spatie/laravel-welcome-notification`, `spatie/laravel-csp`, `spatie/laravel-health` | (unchanged — all correctly named) | Yes, all | Various, all active | Presumed yes | Presumed yes | **VERIFIED (high confidence, not individually re-checked live in this pass)** | These are all long-established, well-known Spatie packages whose names and general purpose this auditor has high confidence in independently of file 18. **This audit did not re-run a live Packagist check on each of these 23 individually** — time was prioritized on the packages flagged as suspicious (above) and on the platform-version-critical ones (Filament/Livewire/PHP). Before a real Composer install, run `composer why-not` / a fresh `composer require --dry-run` pass on this list rather than trusting either file 18 or this report blindly. |
| `spatie/laravel-event-sourcing`, `spatie/laravel-comments`, `spatie/laravel-model-states`, `spatie/laravel-newsletter`, `spatie/laravel-fractal`, `spatie/valuestore`, `spatie/laravel-view-models`, `spatie/laravel-geocoder`, `spatie/laravel-menu`, `spatie/laravel-feeds`, `wireui/wireui`, `spatie/laravel-ray` | (unchanged) | Yes, all real | — | N/A (rejected) | N/A | **VALID BUT DEFERRED / CORRECTLY REJECTED** | These are all real packages that the architecture explicitly chose *not* to use, with stated rationale in file 18 §2.5. This audit did not find fault with the rejection reasoning (event sourcing contradicts the stated ADR against it; comments/newsletter have real alternatives already chosen; etc.). Preserve these rejections — do not re-litigate without new information. |
| `drewm/mailchimp-api`, `guzzlehttp/oauth2-client`, `magentron/laravel-firewall-filament`, `stancl/tenancy`, `codewithdennis/larament`, `rappasoft/laravel-livewire-tables`, `pxlrbt/filament-activity-log`, `alexjustesen/filament-spatie-laravel-activitylog` | — | Not independently re-verified | — | — | — | **UNVERIFIED (out of scope for this pass)** | These appear only in passing / exploratory contexts in the earlier discovery documents (not in file 18's canonical register), generally as "considered and not adopted." Given they are not in the current package matrix, they were deprioritized. If any of these resurface as a live dependency in future work, verify before adopting — do not assume file 9/12/13's mention of them constitutes verification. |
| `filament/spatie-laravel-activitylog-plugin` | Unresolved — file 18 itself flags this as **UNVERIFIED** (B5) | Unconfirmed | — | — | — | **UNVERIFIED** | This audit could not confirm this exact package name either. Filament v4/v5-compatible activity-log integration needs a fresh check — `pxlrbt/filament-activity-log` (which appears elsewhere in the archive's exploratory documents) may be the actual current answer, but this was not independently confirmed live in this pass. **Do not install either name without checking Packagist directly first.** |

### 5.1 Summary

- **Confirmed fabricated:** `minimolabs/laravel-webpush` (1)
- **Wrong name, real package exists under a different name:** `bezhansalam/filament-shield` → `bezhansalleh/filament-shield`; `filament/tiptap-editor` → `awcodes/filament-tiptap-editor` (deprecated, use native RichEditor instead) (2)
- **Likely fabricated / could not confirm, contrary to file 18's more cautious "unverified" framing:** `spatie/laravel-rate-limits` (1)
- **Genuine compatibility blockers correctly caught by file 18 and independently confirmed here:** Filament v3 and Livewire v3 (2)
- **Still genuinely unverified after two audit passes:** `filament/spatie-laravel-activitylog-plugin` (1)
- **A meaningful new fact the archive predates:** Laravel 13's Fortify now ships first-party passkey support, which may change the `laravel/breeze` vs. starter-kit decision.

---

## 6. Requirements Inventory (Phase 5)

Extracted from the business layer (discovery1) and cross-checked against the domain/platform layer. This is a summary, not a reproduction of the source documents.

**Core education:** institutions, campuses (implied but not explicitly modeled as a separate entity — flag: campus-level granularity within a single institution does not appear to have its own entity anywhere in the domain catalogue; if multi-campus institutions are a real requirement, this is a **missing requirement**, not just an implementation detail), programmes, disciplines, qualification/award level, admissions, admission cycles, admission requirements/policies, qualifications (reference data), scholarships, education authorities, accreditation, locations, study abroad (explicitly stress-tested in file 11 §8, with some concepts noted as "not yet modelled" — see §17 below).

**Content:** news, guides, project topics, seminar topics, research materials — modeled as one "Educational Resource" family with distinct subtypes, correctly avoiding both a god-table and needless entity proliferation (see C7 above). Exam preparation content is referenced (`resource/exam-prep` URL pattern appears in the archive) but its relationship to the four-type content model is not explicitly nailed down anywhere this audit found — worth a direct check before implementation.

**Discovery:** Typesense-backed search, faceting, filtering, sorting, autocomplete, discovery pages, institution-scoped programme pages, cross-institution programme hubs, SEO landing pages — all present and discussed in depth (file 14 §9–11, §27–28).

**Users:** authentication, profiles, settings, preferences, notification preferences — present, with settings correctly separated by scope in file 17 §23 (see §9 below).

**RBAC:** platform admins, institution memberships, multiple admins per institution, institution-scoped permissions, dynamically manageable roles — present via Spatie Permission + custom scoping pivot, though the exact role list is inconsistent across documents (C5).

**Engagement:** likes, saves/bookmarks, comparisons, sharing — present (file 14 §12–13, §16). Follows are mentioned but their justification (vs. saves) is not strongly argued anywhere — worth confirming this isn't feature duplication.

**Notifications:** in-app/database, email, web push, notification preferences — present; SMS/phone is mentioned in passing in the requirements sweep of discovery1 but this audit did not find it substantively architected anywhere in discovery2. **If SMS is a real product requirement, it is currently unaddressed in the architecture**, not just deferred — worth explicitly confirming whether it's in scope.

**Community:** questions, answers, discussions, tags, voting, accepted answers, moderation, reporting — present (file 14 §14, file 13 §10), explicitly framed as a Stack Overflow + Reddit hybrid per the task's own stated direction.

**Communication/marketing:** newsletters (Mailchimp explicitly rejected as a lock-in risk per ADR-022-3 — a defensible call), spam prevention (honeypot), webhooks (incoming via `spatie/laravel-webhook-client`, outbound deferred), social sharing (client-side only, per ADR-25, explicitly rejecting server-side share-count tracking as unnecessary — a reasonable simplicity call).

**Data:** imports (from external sources — the earlier discovery documents describe this extensively, e.g., JAMB/WAEC/NECO/NABTEB source ingestion), exports (GDPR personal data export via Spatie), synchronization, backups (`spatie/laravel-backup`). This layer looks the least concretely specified of all the requirement areas relative to how central it is to the product's value proposition ("Acquire Educational Information" is Business Capability #1) — the archive is much stronger on how data is *modeled and searched* than on how it is *acquired and kept current*, which is a real gap given the business layer explicitly names data acquisition as core capability #1.

**No invented requirements were added to this list beyond what appears somewhere in the archive.**

---

## 7. Domain Architecture Findings (Phase 6)

**What StudyNexus actually needs, challenged item by item:**

- **5 aggregate roots (Educational Institution, Programme, Scholarship, Admission Cycle, Information Source):** Defensible. Each has its own invariant boundary, its own lifecycle, and cross-aggregate operations are explicitly listed as forbidden knowledge (e.g., Institution must not know Scholarship targeting). File 10's own adversarial pass concluded "all 5 aggregate boundaries are correctly drawn," and this audit did not find a case for merging or splitting any of them that the archive hadn't already considered and rejected with a stated reason.
- **1 domain service (`InstitutionMerger`):** Justified — genuinely needs to coordinate two Institution aggregates and their child Programmes in one transaction; no single aggregate can own this. This is the correct, minimal use of a domain service, not a smell.
- **No repositories:** The archive explicitly considered and rejected the repository pattern (file 10 §6), reasoning that Eloquent-as-persistence is a pragmatic Laravel idiom and the one place a repository-like seam is genuinely needed (`InstitutionMerger`) is already handled as a domain service. This is a sound, non-dogmatic call — repositories-for-repositories'-sake would have been the over-engineering this audit was asked to watch for, and the archive avoided it.
- **No CQRS, no event sourcing:** Explicitly rejected (`spatie/laravel-event-sourcing` is in the rejected list, citing a contradicting ADR). Given the product is a content/search/discovery platform, not a financial ledger or workflow engine, this is the right call — event sourcing here would be pure ceremony.
- **20–23 application actions:** See C4 — the count itself is unreconciled, but the *shape* (thin Actions invoking aggregate methods, used for orchestration/multi-entry-point cases, not for simple CRUD) matches Laravel Beyond CRUD idiom correctly. File 10's own challenge removed 4 actions it judged unjustified (e.g., actions that didn't actually need to exist as a separate class from a simple aggregate method call) — this is exactly the kind of self-correction the task asked this audit to check actually happened, and it did happen, even if the resulting number was never cleanly re-published.
- **27–29 value objects / ~13–16 enums:** See C2. Individually, the named VOs (Monetary Amount, Duration, Location, Validity Period, Admission Rule, etc.) are genuine value objects with real equality semantics and immutability requirements — not entities wearing a VO costume. This part of the modeling is sound; only the bookkeeping (the total count) is broken.
- **Generic/god abstractions were considered and explicitly rejected**, per file 16 §24's own anti-pattern list: no generic `EducationalOpportunity` entity, no generic `UserInteraction` entity (each engagement type gets its own model instead), no `ForeignInstitution`/`VisaRequirement` entity (treated as reference data, not domain). These are all correct calls for a platform at this stage — generic abstractions would only pay off with far more scale/variance than a single-country MVP has.

**Overall verdict on domain architecture:** Sound in substance, sloppy in bookkeeping. An implementation team should re-derive the exact final counts (VOs, events, actions) directly from the class-level definitions in files 9 and 11 rather than trusting any header number in the archive.

---

## 8. Database / Data Model Findings (Phase 7)

- **Polymorphic relationships:** Used deliberately and narrowly — `AdmissionPolicy` belongs to a polymorphic "Governable" (Institution or Programme), and `AccreditationRecord` belongs to a polymorphic "Accreditable" (Institution or Programme). This is a reasonable, minimal use of polymorphism (two concrete types, not an open-ended set), not the kind of over-general polymorphic sprawl the audit was asked to watch for.
- **Reference data vs. domain entities:** `Qualification`, `Scholarship Provider`, and `Education Authority` are correctly modeled as supporting/reference entities (auto-increment IDs, simple active/inactive lifecycle) rather than full aggregates with UUIDs and rich invariants. File 16 explicitly records a correction moving `Qualification` out of a prior Reference→Domain FK violation, which is a legitimate fix, not scope creep.
- **Uniqueness and lifecycle:** Each aggregate's invariants (e.g., "one active admission policy per cycle per parent," "unique accreditation per authority per period") are stated as explicit invariant IDs (INV-I1…INV-S3), which is good practice — but this audit did not find matching database-level unique constraints or index definitions cross-referenced against every one of these invariants. That translation (invariant → migration constraint) appears to happen in the Laravel architecture section of file 11 (§11) but was not exhaustively checked invariant-by-invariant in this pass. **Recommend a dedicated pass matching every INV-* to an actual migration constraint before writing migrations.**
- **Multi-country/globalization:** The business layer is explicit that geographic expansion beyond Nigeria comes only after the Nigerian model is proven, and that the data model should "anticipate" this without "optimizing prematurely" (discovery1 file 01). The domain architecture's Location value object (country/region/city, ISO 3166-1 for country) is a reasonable minimal accommodation. File 11 §8–9 explicitly stress-tests Study Abroad and future-education-expansion scenarios and **honestly documents which Study Abroad concepts are not yet modeled** rather than pretending completeness — this is a genuine strength of the archive's process discipline, worth preserving as a pattern (explicit "not yet modeled" lists) in the canonical set.

---

## 9. RBAC and Settings Findings (Phase 9)

**RBAC:** The mechanism — Spatie Permission for global roles, a custom `OrganizationMember` pivot for institution-scoped roles, Filament Shield for admin-panel-resource-level authorization, Sanctum for API tokens, Laravel Policies for object-level checks — is a coherent, layered design that correctly separates concerns ("who can access this Filament resource" vs. "which organizations does this user belong to, with what role" vs. "does this token have this scope"). This matches the actual requirement (platform admins ≠ institution admins ≠ equal privileges within an institution) rather than over- or under-fitting it. The unresolved part is purely the concrete role list/count (C5), which is a product decision, not an architecture defect.

**Settings:** File 17 §23 explicitly separates configuration, user preferences, notification preferences, authorization, and domain state into distinct concerns rather than one settings blob — this is the correct separation per the audit's own Phase 9 criteria, and `spatie/laravel-settings` (confirmed real and current) is an appropriate tool for the configuration/preference layers specifically (not for domain state, which correctly stays in the domain model instead).

---

## 10. Notification Findings

Architecture: database (in-app), email, web push (`laravel-notification-channels/webpush`, confirmed real — see §5), and notification preferences per user. SMS is named as a requirement in the business-layer requirements sweep but this audit found no corresponding architectural section — see §6. No custom notification framework was built; Laravel's native notification system is used directly, which file 16 explicitly lists as a correction away from an earlier, unnecessary "custom notification framework" idea (§24 item 13) — a good simplicity call.

---

## 11. Engagement Findings

Likes, saves/bookmarks, comparisons, and client-side-only sharing are each modeled as their own distinct mechanism rather than a single generic "interaction" entity (explicitly rejected per §24 item 9). This is the right level of granularity for a handful of genuinely different engagement types with different semantics (a "like" and a "save" are not interchangeable data), and avoids the generic-interaction-entity anti-pattern the audit was asked to check for.

---

## 12. Community Findings (Phase 10)

The Stack-Overflow-plus-Reddit hybrid direction is present in file 13 §10 and file 14 §14, covering questions, answers, discussions, tags, voting, and moderation. `spatie/laravel-comments` was explicitly evaluated and rejected as "too simple for SO+Reddit hybrid; paid license" — both parts of that rationale are checkable facts this audit did not independently re-verify (Spatie's comments package licensing model), but the reasoning as stated is coherent and not obviously wrong. No over-engineering was found here beyond the general observation that "community" is architecturally sketched at a high level relative to how much surface area a real Q&A/discussion feature actually has (accepted answers, reputation, flagging workflows, moderation queues) — this section reads more like a capability list than a fully pressure-tested design, unlike the domain core, which had multiple adversarial passes. **Recommend a dedicated adversarial pass on Community specifically before implementation, mirroring what was done for the domain core.**

---

## 13. Content Findings (Phase 11)

Already covered under C7 — the four-type Educational Resource model (Guide, Project Topic, Seminar Topic, Research Material) plus a separate News type is the right shape: neither a god-table nor per-URL entity proliferation. One loose end: the relationship between "exam preparation" content (referenced via a `resource/exam-prep` URL pattern found in the archive) and this four-type model was not clearly pinned down in the documents this audit reviewed — confirm whether exam-prep is a fifth Educational Resource subtype or something else before building the content schema.

---

## 14. Search Findings (Phase 8, search half)

Laravel Scout with Typesense as the search engine is architecturally separated from the primary database correctly in principle (search index described as a projection, not the source of truth, in file 11 §14 and file 14 §11/§27). **Independently confirmed via live search:** `laravel/scout` v4.0.0 has native, first-party Typesense support as of Laravel 13's documentation — no third-party Scout driver package is needed, which simplifies the dependency list slightly relative to what some earlier discovery documents may have assumed (the `typesense/laravel-scout-typesense-driver` community package explicitly recommends switching to Scout's native driver in its own README). Indexing/rebuild/failure-recovery are discussed but this audit did not find a fully concrete "what happens when Typesense is down or the index is stale" runbook — that's an operational gap worth closing before production, not an architectural one.

---

## 15. SEO Findings (Phase 8, SEO half)

The archive correctly distinguishes server-rendered, crawlable pages from client-side Typesense search results — file 14 §11 and §27–28 explicitly discuss canonical URLs, controlled indexation (rejecting "every facet combination as an SEO page," per §24 item 20, a good anti-pattern catch), sitemap strategy, and a defined public-site information architecture. This is a genuine strength; the archive did not fall into the common trap of treating client-side search as the SEO strategy, and it explicitly says so.

---

## 16. Samphina-Derived Capability Findings (Phase 12)

File 14 §20 ("Samphina-Style Capability Check") exists as a dedicated section, which is the right instinct — evaluate against a competitor rather than copy it. This audit did not independently verify Samphina's actual current feature set (out of scope and not central to StudyNexus's own architectural correctness), but the archive's framing — checking whether each Samphina-inspired idea is already covered by an existing StudyNexus concept before adding something new — is the correct methodology per this audit's own Phase 12 instructions, whatever the specific conclusions turned out to be.

---

## 17. Missing Requirements

1. **Multi-campus institutions** — no explicit entity or attribute for this; may be a gap if real institutions in scope have multiple physical campuses under one administrative Institution.
2. **SMS notifications** — named in the business-layer requirements sweep, absent from the notification architecture.
3. **Data acquisition / ingestion pipeline** — the single most business-critical capability ("Acquire Educational Information," Capability #1) is the least concretely architected relative to search, content, and engagement, which all received deep, repeated adversarial passes.
4. **Study Abroad — explicitly self-flagged as incomplete** by the archive itself (file 11 §8, "Study Abroad Concepts Not Yet Modelled") — worth listing here precisely because the archive's own honesty about this gap should be preserved, not lost, in any consolidated document.
5. **Exam-prep content type placement** — unclear whether it's a fifth content subtype or something else.

---

## 18. Over-Engineering Findings

Notably, this audit found **less** over-engineering than the task prompt's framing anticipated. The repository pattern, CQRS, event sourcing, a generic interaction entity, a generic content god-table, ABAC, database-level multi-tenancy, ad-hoc state-machine packages, and ad-hoc newsletter integration were all explicitly considered and explicitly rejected with stated reasons, and this audit did not find fault with those rejections. The genuine problems in this archive are **inconsistency and unverified claims**, not over-engineering. The one candidate for mild over-engineering is the **Spatie Laravel Data "five-type problem"** flagged by file 10 §9 itself ("Correct principle, insufficient concrete guidance") — worth a concrete resolution (a written rule for exactly when something is a Data DTO vs. a domain Value Object) before it causes inconsistent usage across a real codebase.

---

## 19. Redundant / Superseded Document Analysis (Phase 13)

| Category | Documents |
|---|---|
| **A. Essential source documents (keep, treat as authoritative)** | `discovery1/01, 02, 03` (business identity/glossary/decisions); `discovery2/11` (frozen domain catalogue, *with the count corrections from §4 applied*); `discovery2/14` (final platform/capability architecture); `discovery2/17` (final corrected blueprint); `discovery2/18` (package verification — *with the additional corrections from §5 applied*) |
| **B. Historical audit documents (keep for record, not for direct implementation reference)** | `discovery1/07, 08, 09, 10`; `discovery2/10` (domain validation — useful to understand *why*, superseded *what* by file 11); `discovery2/16` (reconciliation — useful process record, but contains the stale Laravel 12 command, §C1) |
| **C. Superseded documents** | `discovery2/09` (superseded by 11); `discovery2/12, 13` (superseded by 14); `discovery2/15` (superseded by 17) |
| **D. Truly redundant / exploratory, safe to archive without loss** | `discovery2/01, 02, 03, 04, 05, 06, 07, 08` — early domain discovery drafts whose conclusions were carried forward and improved in files 9–11. Nothing in this audit's review of files 9–18 suggested a decision unique to files 1–8 was lost; they read as genuine drafts, not as containing an orphaned requirement. (This was not exhaustively verified sentence-by-sentence given the ~87,000-word size of this subset — a final check before deletion/archiving would be prudent, not a full re-read.) |
| **`discovery1/05-adrs.md`** | Effectively empty (30 words) — either a placeholder that was never filled in, or content that migrated into `discovery2`'s later ADR references (file 10, file 11 §2 both discuss numbered ADRs that don't appear to trace back to this file). **Flag as a likely gap**, not confirmed redundant. |

---

## 20. Canonical Document Recommendation

A future implementation pass should work from **no more than these 9 documents**, each explicitly corrected per this audit before use:

1. `discovery1/01-business-overview.md` (as-is)
2. `discovery1/03-approved-decisions.md` (as-is)
3. `discovery1/08-traceability-matrix.md` (as-is)
4. `discovery2/11-domain-architecture-frozen-baseline.md` — **with §10's VO/event/action counts re-derived and reconciled per C2/C3/C4 before use**
5. `discovery2/14-final-platform-and-capability-architecture.md` (as-is, cross-checked against #4)
6. `discovery2/17-final-corrected-pre-implementation-blueprint.md` (as-is — this is the document that got the platform version right)
7. **A new, single "Package Register v2" document** — starting from `discovery2/18` and applying every correction in §5 of this report (remove/replace `spatie/laravel-rate-limits`, resolve `filament/spatie-laravel-activitylog-plugin`, pick Filament v4 or v5 explicitly, pick Livewire v4 explicitly, decide on the native-Fortify-passkeys question, drop the redundant Typesense Scout driver package)
8. **A new "RBAC Decision" document** — the concrete, final role list, resolving C5 in one place instead of three
9. `discovery1/04-open-questions.md` — kept live and updated, not archived, since several of its listed open questions (persona priority, foreign-institution scope, decision-support definition) remain genuinely open and relevant to decisions made later in the chain

---

## 21. Critical Blockers

1. **Filament v3 / Livewire v3 code samples throughout files 11–17 will not run on the stated Laravel 13 baseline.** No updated v4/v5-idiomatic samples exist anywhere in the archive. (§C6, §5)
2. **`spatie/laravel-rate-limits` does not appear to exist under that name** and is currently listed as "evaluate later" rather than removed — if left in a dependency list, `composer require` will fail. (§5)
3. **The concrete RBAC role list is not settled** — three different documents imply three different shapes. (§C5)
4. **The VO/event/action counts in the "frozen" catalogue don't add up internally** — before generating enum classes or event listeners from this catalogue, someone needs to do the actual count. (§C2–C4)
5. **The data-acquisition/ingestion architecture — the platform's core stated value proposition — is the least developed part of the entire archive.** (§6, §17)

---

## 22. Recommended Next Steps

1. Resolve the archive-numbering discrepancy (§2.3) with whoever produced these documents, if traceable — confirm whether a Document is genuinely missing.
2. Do a literal, mechanical recount of value objects, domain events, and application actions directly from class-level definitions in files 9 and 11; publish one corrected catalogue.
3. Pin the Filament major version (v4 or v5) and Livewire v4 as an explicit, dated decision, and regenerate the affected code samples — or at minimum, mark every existing sample in files 11–17 as "written against a pre-Filament-v4 API, needs updating" so no one copies it as-is.
4. Settle the RBAC role list once, in one document.
5. Independently verify (via `composer require --dry-run` against a real Laravel 13/PHP 8.5 project) the ~23 Spatie packages this audit flagged as "high confidence but not individually re-checked live" (§5.1) before relying on this report as final proof of their correctness either.
6. Resolve `filament/spatie-laravel-activitylog-plugin` vs. `pxlrbt/filament-activity-log` as the actual Filament-v4/v5-compatible activity log integration.
7. Give the data-acquisition/ingestion pipeline the same depth of adversarial review the domain core and search/SEO layers already received — it is the most business-critical and least architected part of the system.
8. Only after 2–7: assemble the 9-document canonical set in §20 and proceed to implementation planning.

**Do not resume implementation from this archive as-is. The above should be closed first.**

---

*End of forensic audit. No StudyNexus implementation, migration, or new "final architecture" was produced. This document reports findings; it does not make product decisions on the owner's behalf.*
