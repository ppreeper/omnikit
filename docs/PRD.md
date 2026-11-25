# Product Requirements Document

**Product:** OmniKit (working name)  
**Document version:** 1.0  
**Status:** Draft for review  
**Date:** 2026-10-18  
**Owners:** Product and engineering  
**Related documents:** [Competitive Analysis](./competitive-analysis.md) · [Tool Equivalence Map](./tool-map.md) · [Technical Specification](./technical-spec.md)

## 1. Executive summary

OmniKit is a self-hosted web application that puts a large catalogue of everyday utilities behind one fast, private, consistent interface. Users convert, format, compare, generate, edit and inspect text, data, code, images, audio, video and documents without uploading files to a third party.

The market is already served by three mature products, analysed anonymously in the [Competitive Analysis](./competitive-analysis.md). Together they expose **233 tool instances** that resolve to **199 distinct user outcomes**. Only **nine outcomes** are present in all three. The rest are fragmented: 82.9% of the combined catalogue appears in only one product.

OmniKit's proposition is completeness without compromise:

- **Complete** — implement all 199 canonical outcomes.
- **Private** — process files and data in the browser by default; no account required.
- **Fast** — static-first delivery, instant search, lazy-loaded heavy tools.
- **Ownable** — one container, no mandatory external services, permissive licence.
- **Coherent** — one tool shell, one design system, one definition of done.

This document defines the product requirements, release scope, acceptance criteria, non-functional targets and success measures. The [Tool Equivalence Map](./tool-map.md) is the authoritative catalogue; this document defines how the product must behave.

## 2. Problem and opportunity

### 2.1 User problem

Everyday digital work creates long-tail tasks that are too small for a dedicated application but too frequent to ignore:

- A developer needs to decode a JWT, compare two JSON payloads, generate a UUID and fix a cron expression in the same afternoon.
- An operations engineer needs to subnet an IPv4 range, hash a value, inspect a certificate-adjacent PDF signature and convert a config file.
- A knowledge worker needs to merge PDFs, compress a photo, convert a spreadsheet and clean a list of duplicates.
- A privacy-conscious organisation cannot send internal documents to an ad-supported online converter.

Existing answers are fragmented across single-purpose websites, desktop utilities and developer scripts. They differ in quality, privacy posture, offline behaviour and file-size limits. Users pay with attention, uploads and trust.

### 2.2 Opportunity

A single self-hosted application can own the long tail if it is:

1. Broad enough that users stop searching elsewhere.
2. Private enough for internal documents and credentials-adjacent data.
3. Fast enough to beat a search engine result.
4. Deployable in minutes by an individual or a small IT team.
5. Maintainable enough that adding the 200th tool is routine, not heroic.

### 2.3 Market evidence

| Finding | Implication |
|---|---|
| 199 distinct outcomes across three products | The category is broad but fragmented |
| Only 9 outcomes shared by all three | A universal core exists and is non-negotiable |
| 101 outcomes unique to one product | Breadth is the largest differentiator |
| Best offline support is limited to one product | Reliability is an open opportunity |
| Only one product combines a PDF suite with server-rendered content and a CMS | Integrated document and content depth is an open opportunity |
| Only one product invests in SEO and content | Discoverability is an open opportunity |
| Only one product requires a backend | Local-first architecture can be a privacy advantage |

## 3. Vision, mission and principles

### 3.1 Vision

Make OmniKit the default self-hosted toolbox for everyday digital tasks: one place, every common tool, no uploads, no lock-in.

### 3.2 Mission

Deliver a complete, accessible, offline-capable utility suite that runs anywhere a container runs and treats user data as private by default.

### 3.3 Product principles

| Principle | What it means in practice |
|---|---|
| **Local-first** | Core tools execute in the browser. Data leaves the device only when the user explicitly uses an optional server feature. |
| **Complete** | Every canonical outcome from the competitive set is implemented, not approximated by a link or a narrower mode. |
| **One shell** | Every tool uses the same layout, input, action, result and error patterns. |
| **Discoverable** | Search, categories, favourites, recents and examples make 199 tools feel navigable. |
| **Fast by default** | The home page and text tools never pay for media, PDF, OCR or WASM bundles. |
| **Accessible** | Keyboard-first operation and WCAG 2.2 AA are release criteria, not enhancements. |
| **Recoverable** | Invalid input produces actionable messages; large jobs can be cancelled; no tab crashes by design. |
| **Self-hostable** | A single container with sensible defaults; optional modules never become hidden dependencies. |
| **Clean-room** | No copied code, copy, assets, translations or sample data from the reference products. |
| **Permissive** | Prefer MIT/Apache-2.0 dependencies and ship under a permissive licence. |

## 4. Users and personas

### 4.1 Primary personas

**P1 — The developer**
- Works with APIs, configuration, logs and identifiers daily.
- Needs JSON/YAML/XML conversion, JWT and hash tools, regex, cron, UUID/ULID, diff, Base64 and URL handling.
- Values keyboard access, exact output, no surprises and offline use.
- Success: completes a task without leaving the keyboard or opening a scratch script.

**P2 — The IT generalist / homelab operator**
- Runs self-hosted services and handles small operational tasks.
- Needs subnet maths, hashing, QR codes, password generation, PDF and image utilities, file conversion and config transforms.
- Values simple deployment, no cloud dependency and predictable resource use.
- Success: deploys once and uses it weekly for mixed tasks.

**P3 — The knowledge worker**
- Handles documents, spreadsheets, images and text.
- Needs PDF merge/split/compress, image resize/compress/convert, CSV cleanup, text comparison, list cleaning and date calculations.
- Values clear labels, examples, drag-and-drop, mobile support and no sign-up.
- Success: finishes an everyday task without installing software or uploading a confidential file.

**P4 — The privacy-conscious organisation**
- Cannot send internal data to public converters.
- Needs self-hosting, no telemetry by default, auditable dependencies and offline operation.
- Values transparency, CSP, container hardening and data-retention guarantees.
- Success: approves the product for internal use and can prove data never leaves the network.

### 4.2 Secondary personas

- **Contributor** — adds a tool using the registry contract and passes the definition of done.
- **Translator** — supplies locale files with typed keys and coverage checks.
- **Administrator** — optionally enables content, analytics or authentication modules.

### 4.3 Jobs to be done

| Job | Desired outcome |
|---|---|
| "I need to inspect or transform a value" | Get a correct result in seconds, with the option to copy or download it |
| "I need to clean or compare content" | See differences and transformations clearly, without losing data |
| "I need to process a file privately" | Finish the job locally with visible progress and no upload |
| "I need to find the right tool" | Reach the correct tool in under 10 seconds by search or category |
| "I need this to work on my server" | Deploy one container and keep working offline |
| "I need to trust the result" | See deterministic behaviour, validation and clear limitations |

## 5. Goals and non-goals

### 5.1 Goals

| ID | Goal | Measure |
|---|---|---|
| G-01 | Complete functional parity | 199/199 canonical outcomes implemented and discoverable |
| G-02 | Excellent discovery | 90% of usability-test participants find a named tool in under 10 seconds |
| G-03 | Private by default | Zero network requests carrying user content in core tools |
| G-04 | Fast | Home LCP ≤ 2.0 s on a mid-range mobile profile; search results ≤ 50 ms |
| G-05 | Reliable | Tool success rate ≥ 99% for valid inputs; crash-free sessions ≥ 99.5% |
| G-06 | Accessible | WCAG 2.2 AA conformance for all P0 and P1 tools |
| G-07 | Offline-capable | All P0 tools usable offline after first visit |
| G-08 | Easy to self-host | Running container in under 5 minutes with one command |
| G-09 | Maintainable | A new simple tool can be added in under one day by a contributor |
| G-10 | Trusted | No critical or high security findings in release audit |

### 5.2 Non-goals

OmniKit is **not**:

- A cloud storage or file-sharing service.
- A collaborative document editor or real-time co-editing product.
- A full IDE, image editor or video production suite.
- A replacement for specialised professional software such as Photoshop, Premiere or Acrobat Pro.
- A general-purpose AI assistant or model-hosting platform.
- A password manager or secrets vault.
- A public SaaS with mandatory accounts, billing or multi-tenant administration.
- A mirror of any reference product's branding, copy, taxonomy or visual design.

### 5.3 Explicit constraints

- Core functionality must work without a database, account or external API.
- Client-side processing is the default for all user content.
- Reference project names must not appear in product documentation or UI.
- No GPL-licensed code may be copied into the codebase.
- The product must remain usable on current Chrome, Firefox, Safari and Edge.
- Heavy media and PDF engines must be lazy-loaded and never block the core bundle.

## 6. Scope and definition of parity

### 6.1 Scope model

| Layer | Description | Required |
|---|---|---|
| Core platform | Shell, routing, registry, search, categories, favourites, recents, theming, i18n, PWA, error handling | Yes |
| Core tools | 33 P0 outcomes covering universal and high-frequency tasks | Yes, release 1.0 |
| Parity tools | 56 P1 outcomes completing two-reference coverage and major category depth | Yes, release 1.1 |
| Long-tail tools | 110 P2 outcomes including advanced media, niche generators and reference data | Yes, release 1.2+ |
| Optional modules | Blog/CMS, contact form, analytics, authentication, server-side processing | Optional, disabled by default |

### 6.2 Definition of complete parity

Parity is achieved when:

1. All **199** canonical outcomes in the [Tool Equivalence Map](./tool-map.md) are implemented.
2. All **233** source tool instances have a documented equivalent.
3. Every partial outcome is upgraded to a first-class implementation.
4. Platform capabilities from all references are present: search, categories, favourites, recents, examples, per-tool state, themes, i18n, offline, SEO and optional content.
5. Every tool meets the definition of done in section 18.
6. No reference product name, copy, asset or code is used.

### 6.3 Release scope

| Release | Theme | Outcomes | Categories |
|---|---|---:|---|
| **1.0 Core** | Universal + high-frequency | 33 | All 13 represented |
| **1.1 Parity** | Two-reference coverage and depth | +56 (89 total) | All 13 |
| **1.2 Long tail** | Complete catalogue | +110 (199 total) | All 13 |

P0 composition:

- **Text (7):** case conversion, find/replace, remove duplicate lines, text statistics, text diff, slugify, Markdown to HTML.
- **Data (6):** JSON format/validate, JSON minify, JSON diff, JSON to CSV, CSV/TSV to JSON, XML format.
- **Encoding (7):** Base64 text, URL encode/decode, hash generator, UUID, password generator, JWT parser, QR code.
- **Developer (2):** regex tester, cron builder.
- **Date/time (1):** Unix timestamp converter.
- **Numbers (2):** percentage calculator, math evaluator.
- **Design (1):** colour converter.
- **Images (4):** resize, compress, crop, JPG to PNG.
- **PDF (3):** merge, split, compress.

## 7. Information architecture

### 7.1 Canonical categories

| Category | Canonical outcomes | Primary audience |
|---|---:|---|
| Text & strings | 35 | All |
| Data formats | 35 | Developers, analysts |
| Encoding & security | 24 | Developers, IT |
| Web & developer utilities | 13 | Developers |
| Date & time | 12 | All |
| Numbers & math | 16 | All |
| Network | 6 | IT, developers |
| Design | 1 | Designers, developers |
| Lists | 11 | All |
| Images | 16 | Knowledge workers, creators |
| Video | 14 | Creators |
| Audio | 5 | Creators |
| PDF & documents | 11 | Knowledge workers |

### 7.2 Navigation model

- **Home:** search, category grid, favourites, recent tools, newest tools, featured tools.
- **Category:** filtered list with count, description and tool cards.
- **Tool:** dedicated route with breadcrumb, title, description, examples, input, actions, result and related tools.
- **All tools:** alphabetical and category views with search.
- **Static pages:** about, privacy, help, licence, changelog.
- **Optional:** blog index, article, contact, admin.

### 7.3 URL model

- `/` — home
- `/tools` — all tools
- `/category/[slug]` — category listing
- `/tools/[slug]` — tool page
- `/favourites` — local favourites
- `/recent` — local recents
- `/about`, `/privacy`, `/help` — static pages
- Optional: `/blog`, `/blog/[slug]`, `/contact`, `/admin/*`

Tool slugs are stable, lower-case, hyphenated and never reused for a different outcome.

### 7.4 Discovery rules

- Search matches title, description, keywords, aliases and category.
- Fuzzy matching tolerates typos and missing separators.
- Results rank exact title, title prefix, keyword, description, then fuzzy distance.
- Audience filter (General / Developer / All) is optional and persisted.
- Empty search shows categories and favourites, not a blank page.
- Every tool is reachable in at most three interactions from home.

## 8. Functional requirements — platform

Priority: **M** must, **S** should, **C** could. All M requirements are release-gating for 1.0 unless a phase is stated.

| ID | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| FR-PLAT-001 | The application exposes a registry of all canonical tools with stable IDs, slugs, titles, descriptions, categories, keywords, phase and implementation loader. | M | Registry validates against a schema; duplicate IDs or slugs fail the build; every catalogue entry resolves to a component. |
| FR-PLAT-002 | The home page presents search, categories, favourites, recents, newest and featured sections. | M | Each section renders from registry/local state; empty states are meaningful; no layout shift. |
| FR-PLAT-003 | Global search returns ranked results as the user types. | M | Query of 199 tools returns in ≤ 50 ms on reference hardware; title and keyword matches rank above description matches; keyboard navigation works. |
| FR-PLAT-004 | Users can browse by category and by full alphabetical list. | M | Category counts match the registry; all 199 tools appear exactly once in the all-tools view. |
| FR-PLAT-005 | Every tool has a stable, directly addressable route. | M | Deep link loads the tool; title and description are correct; 404 is shown for unknown slugs. |
| FR-PLAT-006 | Users can favourite and unfavourite tools and reorder favourites. | S | State persists locally; drag-and-drop and keyboard reordering both work; reset is available. |
| FR-PLAT-007 | The application records recently used tools locally. | S | Most recent 12 unique tools appear; recents can be cleared; no server transmission. |
| FR-PLAT-008 | Tool input state persists per tool unless the user resets it. | S | Reload restores supported state; reset clears it; sensitive fields can opt out of persistence. |
| FR-PLAT-009 | Every tool provides at least one worked example. | M | Example loads valid input and expected output/behaviour; examples are translated or fall back to English. |
| FR-PLAT-010 | Results can be copied and, where meaningful, downloaded. | M | Copy shows success feedback; download uses a sensible filename and MIME type; failures are announced. |
| FR-PLAT-011 | Users can choose light, dark or system theme. | S | Choice persists; no flash of incorrect theme; contrast meets WCAG AA in both themes. |
| FR-PLAT-012 | The interface is responsive from 320 px to large desktop. | M | No horizontal scrolling at 320 px; touch targets ≥ 44 px; tool controls remain usable. |
| FR-PLAT-013 | Keyboard users can reach every action and open a command palette. | S | Tab order is logical; visible focus; `Ctrl/Cmd+K` opens search; `Escape` closes overlays. |
| FR-PLAT-014 | The interface is localisable with typed keys and English fallback. | S | No hard-coded user-facing strings in components; missing keys fall back and are reported in CI; locale switch persists. |
| FR-PLAT-015 | The application installs as a PWA and works offline for P0 tools after first visit. | M | Manifest is valid; service worker precaches shell and P0 chunks; offline reload works; update prompt is shown. |
| FR-PLAT-016 | Public pages are pre-rendered with unique metadata. | S | Each tool has title, description, canonical URL, Open Graph and Twitter metadata; sitemap includes all public routes. |
| FR-PLAT-017 | Analytics are optional, consent-aware and off by default. | C | No analytics request occurs before opt-in; events contain no user content; self-host can disable entirely. |
| FR-PLAT-018 | Errors are handled consistently and recoverably. | M | Error boundary catches tool crashes; user sees a clear message and reset action; error is logged locally without content. |
| FR-PLAT-019 | The application provides about, privacy, help and licence pages. | M | Pages explain local processing, optional modules, data handling and licence; links are reachable from the footer. |
| FR-PLAT-020 | Self-hosters can configure branding, base path, locales and optional modules without code changes. | S | Environment/config file controls site name, logo, default locale, enabled modules and analytics; invalid config fails fast. |
| FR-PLAT-021 | The application remains usable when JavaScript-heavy features are unavailable. | C | Static content and tool descriptions render without JS; core input forms degrade gracefully where feasible. |
| FR-PLAT-022 | The application exposes a machine-readable catalogue for integrations and tests. | C | Catalogue JSON lists IDs, slugs, categories, phase and keywords; it is generated from the registry. |

## 9. Functional requirements — tool framework

| ID | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| FR-TOOL-001 | Every tool declares metadata: ID, slug, title, summary, category, keywords, aliases, phase, audience, icon and related tools. | M | Schema validation passes; missing required fields fail CI; metadata drives search, routing and listings. |
| FR-TOOL-002 | Tool implementations are lazy-loaded by route. | M | Initial bundle excludes media/PDF/WASM code; loading state is visible; failure to load shows retry. |
| FR-TOOL-003 | Inputs are validated before processing. | M | Invalid input produces a specific message; processing does not start; valid input proceeds. |
| FR-TOOL-004 | Long-running work reports progress and can be cancelled. | M | Progress is visible after 300 ms; cancel stops work and releases resources; UI remains responsive. |
| FR-TOOL-005 | Files are processed locally by default and never uploaded. | M | Network inspection shows no content-bearing request; file input accepts only declared types; object URLs are revoked. |
| FR-TOOL-006 | File size and memory limits are explicit and enforced. | M | Limits are documented in the UI; oversized files are rejected before allocation where possible; guidance is offered. |
| FR-TOOL-007 | Tool output is deterministic for the same input and options. | M | Repeated runs produce identical output except explicitly random tools, which declare randomness. |
| FR-TOOL-008 | Every tool offers copy and, where applicable, download and reset. | M | Actions work with keyboard and pointer; feedback is announced; reset returns to a clean state. |
| FR-TOOL-009 | Tool-specific options are visible, labelled and persisted only when safe. | S | Options have defaults; changes update output; sensitive options are not persisted. |
| FR-TOOL-010 | Tools handle empty, malformed, boundary and very large inputs gracefully. | M | Test fixtures cover each class; no unhandled exception; message explains the problem. |
| FR-TOOL-011 | Tools declare whether they use randomness, network access or workers. | M | Declaration appears in metadata and privacy notes; undeclared network access fails review. |
| FR-TOOL-012 | Tool pages include a short explanation, example and limitations. | S | Content is original, accurate and translated or fallback-safe; limitations are explicit for lossy operations. |

## 10. Functional requirements — tool categories

The full catalogue, IDs and phase assignments are in [Tool Equivalence Map](./tool-map.md). The requirements below define category-level behaviour and acceptance.

### 10.1 Text and strings (35 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-TXT-001 | Provide case conversion across camel, Pascal, snake, kebab, constant, dot, path, sentence, title and upper/lower forms. | M |
| FR-TXT-002 | Provide text comparison with unified and side-by-side views and ignore-case/whitespace options. | M |
| FR-TXT-003 | Provide find/replace with plain-text and regular-expression modes, capture groups and replace-all. | M |
| FR-TXT-004 | Provide text statistics: characters, graphemes, words, lines, sentences, paragraphs and frequency. | M |
| FR-TXT-005 | Provide duplicate-line removal with trim, case-sensitivity and preserve-order options. | M |
| FR-TXT-006 | Provide slug generation with transliteration, separator and length controls. | M |
| FR-TXT-007 | Provide Markdown to sanitised HTML with live preview and export. | M |
| FR-TXT-008 | Provide the remaining P1/P2 text outcomes: reverse, repeat, truncate, split, join, substring, case randomisation, palindrome, Morse, ROT13, rotate, censor, quote, hidden characters, email extraction/normalisation, table conversion, HTML entities, WYSIWYG, binary, NATO, Unicode escape and lorem ipsum. | S/C |

Acceptance: Unicode input is handled by grapheme where the user perceives characters; no tool corrupts emoji or combining marks; regex modes protect against catastrophic backtracking; output preserves line endings unless normalisation is requested.

### 10.2 Data formats (35 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-DAT-001 | Format and validate JSON with line/column errors and optional repair hints. | M |
| FR-DAT-002 | Minify JSON without changing semantics. | M |
| FR-DAT-003 | Compare JSON semantically and textually. | M |
| FR-DAT-004 | Convert JSON to CSV and CSV/TSV to JSON with configurable headers, delimiter and nesting strategy. | M |
| FR-DAT-005 | Format and validate XML with namespace-aware errors. | M |
| FR-DAT-006 | Provide JSON tree viewing, JSON Schema generation and XML/WSDL tree inspection with XPath. | S |
| FR-DAT-007 | Provide JSON, YAML, TOML and XML conversions in every direction listed in the catalogue, plus formatting and viewing. | S |
| FR-DAT-008 | Provide CSV transforms: delimiter change, transpose, column insert/swap, incomplete-record detection, CSV↔TSV/XML/YAML. | S/C |
| FR-DAT-009 | Provide JSON escape/unescape, stringify and key-based sorting. | S/C |
| FR-DAT-010 | Provide phone parsing, IBAN validation, HTTP status and MIME reference lookups. | S/C |

Acceptance: conversions round-trip where the formats permit; lossy conversions warn before execution; large documents do not freeze the UI; CSV parsing supports quoted fields, embedded newlines and configurable dialects; errors identify the offending line and column.

### 10.3 Encoding and security (24 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-ENC-001 | Encode and decode Base64 text and files with correct binary handling. | M |
| FR-ENC-002 | Encode and decode URL components and full URLs. | M |
| FR-ENC-003 | Generate MD5, SHA-1, SHA-2 and SHA-3 digests with hex/Base64 output. | M |
| FR-ENC-004 | Generate and validate UUID v1/v4/v7, with bulk generation and formatting options. | M |
| FR-ENC-005 | Generate passwords with length, character-set and entropy feedback. | M |
| FR-ENC-006 | Decode JWT header and payload without claiming verification. | M |
| FR-ENC-007 | Generate QR codes for text, URL, email, phone, SMS, WiFi and vCard. | M |
| FR-ENC-008 | Provide bcrypt hash/compare, HMAC, AES/TripleDES/Rabbit encryption, RSA key pairs, BIP39, tokens, ULID, OTP, password-strength analysis, basic auth and random port generation. | S/C |

Acceptance: cryptographic operations use audited primitives (WebCrypto or vetted libraries); secrets are never logged, persisted or sent; every security tool states its limits; password and key material can be cleared in one action; JWT decoding clearly warns that signature verification is not performed.

### 10.4 Web and developer utilities (13 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-DEV-001 | Provide a regex tester with live highlighting, groups, replacement preview and common patterns. | M |
| FR-DEV-002 | Provide a cron parser/builder with human-readable explanation and next-run preview in a chosen timezone. | M |
| FR-DEV-003 | Provide SQL formatting with dialect selection. | S |
| FR-DEV-004 | Provide device, meta/Open Graph, keycode, user-agent, safelink, git, regex, chmod, Docker Compose and benchmark helpers. | C |

Acceptance: regex execution is bounded by time and input length; cron previews account for DST; reference data carries a version date; generated snippets are copy-ready and escaped correctly.

### 10.5 Date and time (12 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-TIM-001 | Convert between Unix timestamps and local/UTC dates with second/millisecond handling. | M |
| FR-TIM-002 | Calculate the interval between dates in multiple units. | S |
| FR-TIM-003 | Provide date-time format conversion, leap-year checking, days/hours, seconds/time, decimal time, clock truncation, weekday, Discord timestamps and ETA calculations. | S/C |

Acceptance: timezone and DST behaviour is explicit; invalid dates are rejected; output formats are selectable; calculations never silently assume a timezone.

### 10.6 Numbers and math (16 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-NUM-001 | Calculate percentages in all common forms (of, change, difference). | M |
| FR-NUM-002 | Evaluate mathematical expressions with functions, constants and clear errors. | M |
| FR-NUM-003 | Convert byte/data units using SI and binary conventions. | S |
| FR-NUM-004 | Sum number lists and generate arithmetic/number sequences. | S/C |
| FR-NUM-005 | Provide temperature, integer-base and Roman numeral conversion. | S/C |
| FR-NUM-006 | Provide engineering calculators: Ohm's law, cable voltage drop, sphere area/volume and slackline tension. | C |

Acceptance: floating-point results are formatted predictably; division by zero and domain errors are explained; engineering formulas cite assumptions and units; calculators validate unit selection.

### 10.7 Network (6 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-NET-001 | Calculate IPv4 subnet properties from address and prefix. | S |
| FR-NET-002 | Convert IPv4 representations and expand ranges. | S |
| FR-NET-003 | Generate IPv6 ULA prefixes. | S |
| FR-NET-004 | Generate and look up MAC addresses and vendors. | C |

Acceptance: invalid addresses are rejected; CIDR boundaries are correct; expanded ranges are bounded to prevent browser lockups; vendor data is bundled and dated.

### 10.8 Design (1 outcome)

| ID | Requirement | Priority |
|---|---|---|
| FR-DSN-001 | Convert colours between HEX, RGB, HSL and CMYK with a visual picker and contrast information. | M |

Acceptance: conversions are accurate; alpha is supported where relevant; contrast ratios use WCAG formulas; copied values use the selected format.

### 10.9 Lists (11 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-LST-001 | Remove duplicates and find unique items with configurable comparison. | S |
| FR-LST-002 | Sort, shuffle, reverse and rotate lists. | S |
| FR-LST-003 | Wrap, unwrap, group/chunk, truncate and duplicate list items. | C |
| FR-LST-004 | Find the most popular items in a list. | C |

Acceptance: delimiter and whitespace handling is explicit; sorting is stable where specified; shuffle declares randomness; empty and single-item lists behave correctly.

### 10.10 Images (16 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-IMG-001 | Resize images by pixels or percentage with aspect-ratio locking. | M |
| FR-IMG-002 | Compress images with quality and format controls and a size comparison. | M |
| FR-IMG-003 | Crop images with numeric and visual controls. | M |
| FR-IMG-004 | Convert JPG to PNG, PNG to JPG and images to WebP. | M/S |
| FR-IMG-005 | Rotate, flip and edit images with annotations and adjustments. | S |
| FR-IMG-006 | Compress PNG losslessly and lossily. | S |
| FR-IMG-007 | Change colours/opacity, create transparent PNGs, remove backgrounds, watermark and split images. | C |
| FR-IMG-008 | Extract text from images with OCR. | C |

Acceptance: EXIF orientation is respected or explicitly stripped; colour profiles are handled predictably; output dimensions and quality are reported; large images fail gracefully with guidance; no image leaves the device.

### 10.11 Video (14 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-VID-001 | Trim, merge, resize, rotate and compress video. | S |
| FR-VID-002 | Convert video to GIF. | S |
| FR-VID-003 | Crop, flip, change speed, loop, add audio, extract audio, create GIFs from images and change GIF speed. | C |

Acceptance: processing runs off the main thread; progress and cancel work; codec/container support is stated; unsupported inputs produce actionable errors; output plays in standard browsers; memory limits are enforced.

### 10.12 Audio (5 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-AUD-001 | Trim, merge and loop audio. | C |
| FR-AUD-002 | Change audio speed without unacceptable pitch artefacts, with an option to preserve pitch. | C |
| FR-AUD-003 | Convert between common audio formats. | C |

Acceptance: waveform or time selection is usable; long files stream or chunk; output format and bitrate are reported; cancellation releases resources.

### 10.13 PDF and documents (11 outcomes)

| ID | Requirement | Priority |
|---|---|---|
| FR-PDF-001 | Merge PDFs in a user-defined order. | M |
| FR-PDF-002 | Split PDFs by page ranges or fixed intervals. | M |
| FR-PDF-003 | Compress PDFs with visible before/after size and quality trade-off. | M |
| FR-PDF-004 | Convert images to PDF with page size, orientation and margin controls. | S |
| FR-PDF-005 | Convert PDF pages to PNG/JPG at selectable resolution. | S |
| FR-PDF-006 | Edit PDF pages: rotate, delete, reorder, annotate and fill forms. | S |
| FR-PDF-007 | Rotate pages and protect documents with a password. | S |
| FR-PDF-008 | Extract embedded images and convert PDF to EPUB. | C |
| FR-PDF-009 | Verify PDF signatures and explain the result. | C |

Acceptance: page ranges are validated; encrypted PDFs prompt for a password and never store it; compression reports actual output size; output preserves page dimensions where expected; large documents show progress and can be cancelled; no file is uploaded.

## 11. Non-functional requirements

### 11.1 Performance

| ID | Requirement | Target |
|---|---|---|
| NFR-PERF-001 | Home page Largest Contentful Paint | ≤ 2.0 s on mid-range mobile, 4G |
| NFR-PERF-002 | Interaction to Next Paint | ≤ 200 ms for core interactions |
| NFR-PERF-003 | Tool route interactive | ≤ 1.0 s on desktop after chunk load |
| NFR-PERF-004 | Search response | ≤ 50 ms for 199 tools |
| NFR-PERF-005 | Initial JavaScript (compressed) | ≤ 180 KB for home and text tools |
| NFR-PERF-006 | Per-tool chunk (simple) | ≤ 60 KB compressed |
| NFR-PERF-007 | Media/WASM loading | Lazy, only on tool entry, with progress |
| NFR-PERF-008 | Memory ceiling | Warn before jobs likely to exceed 1 GB; never crash silently |
| NFR-PERF-009 | Offline reload | ≤ 2.0 s for cached shell |

### 11.2 Reliability and compatibility

| ID | Requirement |
|---|---|
| NFR-REL-001 | Current and previous major versions of Chrome, Firefox, Safari and Edge are supported. |
| NFR-REL-002 | Tool crashes are isolated by an error boundary and do not break navigation. |
| NFR-REL-003 | Service worker updates never leave users on a broken mixed-version cache. |
| NFR-REL-004 | All P0 tools have automated unit tests and at least one end-to-end path. |
| NFR-REL-005 | Deterministic tools produce stable snapshots across supported browsers. |
| NFR-REL-006 | Release artefacts are immutable and versioned. |

### 11.3 Accessibility

| ID | Requirement |
|---|---|
| NFR-A11Y-001 | Conform to WCAG 2.2 AA for all P0/P1 pages. |
| NFR-A11Y-002 | All functionality is operable by keyboard, including drag-and-drop alternatives. |
| NFR-A11Y-003 | Focus is visible and managed on route and dialog changes. |
| NFR-A11Y-004 | Form controls have programmatic labels, descriptions and error associations. |
| NFR-A11Y-005 | Status messages and async results use appropriate live regions. |
| NFR-A11Y-006 | Motion respects `prefers-reduced-motion`. |
| NFR-A11Y-007 | Colour is never the sole carrier of meaning. |
| NFR-A11Y-008 | Automated axe checks run in CI; manual audit before 1.0. |

### 11.4 Security and privacy

| ID | Requirement |
|---|---|
| NFR-SEC-001 | Core tools make no network request containing user input, files or results. |
| NFR-SEC-002 | A strict Content Security Policy is shipped; inline script is avoided or nonce/hash-controlled. |
| NFR-SEC-003 | Dependencies are pinned, audited and licence-checked in CI. |
| NFR-SEC-004 | No secrets, tokens or file contents are written to logs. |
| NFR-SEC-005 | Uploaded files are never persisted by default; any optional server feature documents retention. |
| NFR-SEC-006 | HTML output is sanitised; user content is never injected as raw HTML without an explicit preview sandbox. |
| NFR-SEC-007 | Cross-origin isolation is enabled only where required by a tool, with clear explanation. |
| NFR-SEC-008 | Security headers include HSTS, `X-Content-Type-Options`, `Referrer-Policy` and frame protection. |
| NFR-SEC-009 | A vulnerability disclosure path and release security review are documented. |

### 11.5 Privacy and data handling

| ID | Requirement |
|---|---|
| NFR-PRV-001 | No account is required for core tools. |
| NFR-PRV-002 | Telemetry is off by default and opt-in. |
| NFR-PRV-003 | Local preferences are inspectable and clearable. |
| NFR-PRV-004 | No third-party fonts, scripts or CDNs are required at runtime. |
| NFR-PRV-005 | Privacy page states exactly what is stored locally and what optional modules collect. |
| NFR-PRV-006 | Self-hosters can disable all outbound requests. |

### 11.6 Maintainability and operations

| ID | Requirement |
|---|---|
| NFR-MNT-001 | Tool registry and catalogue are generated and validated from one source of truth. |
| NFR-MNT-002 | A simple tool can be added by creating one metadata file and one component. |
| NFR-MNT-003 | TypeScript strict mode is enabled. |
| NFR-MNT-004 | Lint, type-check, unit tests, E2E smoke tests and build run in CI. |
| NFR-MNT-005 | Bundle budgets are enforced in CI. |
| NFR-MNT-006 | Documentation covers deployment, configuration, tool authoring and architecture decisions. |
| NFR-MNT-007 | Logs are structured and contain no user content. |
| NFR-MNT-008 | Health endpoint is available for container orchestration. |

### 11.7 Deployment

| ID | Requirement |
|---|---|
| NFR-DEP-001 | A single container serves the application with sensible defaults. |
| NFR-DEP-002 | Docker Compose example runs in under five minutes. |
| NFR-DEP-003 | Configuration is environment-driven and validated at startup. |
| NFR-DEP-004 | The app works behind a reverse proxy with a configurable base path. |
| NFR-DEP-005 | Optional modules can be enabled independently. |
| NFR-DEP-006 | Images are multi-arch (amd64/arm64) and run as non-root. |
| NFR-DEP-007 | Backup/restore applies only to optional server modules; core is stateless. |

## 12. UX requirements

### 12.1 Tool page anatomy

1. Breadcrumb and category.
2. Title, one-sentence summary and privacy indicator.
3. Examples and "how it works" disclosure.
4. Input area with label, help text, validation and optional file drop zone.
5. Options panel with defaults and reset.
6. Primary action, secondary actions and cancel for long jobs.
7. Result area with copy/download, metadata (size, dimensions, duration) and error state.
8. Limitations and related tools.

### 12.2 Interaction rules

- Primary action is obvious and keyboard-submittable.
- Destructive actions require confirmation or provide undo.
- Long jobs show progress after 300 ms and remain cancellable.
- Results never replace input silently.
- Copy actions announce success to assistive technology.
- Validation appears inline and on submit, not only as a toast.
- Empty states explain what to do next.
- Errors state cause, context and recovery action.

### 12.3 Visual and content rules

- One design system across all tools; no per-tool visual dialects.
- Tool descriptions are original, plain-language and 1–3 sentences.
- Icons are original or permissively licensed with attribution.
- No dark patterns, forced sign-up or interstitials.
- Advertising is out of scope.
- Motion is subtle and respects reduced-motion.

## 13. Internationalisation requirements

| ID | Requirement |
|---|---|
| FR-I18N-001 | UI strings, tool metadata, examples and validation messages are translatable. |
| FR-I18N-002 | English is the source and fallback locale. |
| FR-I18N-003 | Launch locales: English, German, Spanish, French, Portuguese, Chinese, Japanese and Ukrainian. |
| FR-I18N-004 | Locale routing is URL-addressable and persists user choice. |
| FR-I18N-005 | Dates, numbers and file sizes use locale-aware formatting. |
| FR-I18N-006 | CI reports missing and unused keys; a locale below 80% coverage is marked beta. |
| FR-I18N-007 | Right-to-left readiness is maintained in layout primitives, even if no RTL locale ships initially. |

## 14. SEO and content requirements

| ID | Requirement | Priority |
|---|---|---|
| FR-SEO-001 | Every public tool page is pre-rendered or statically generated with unique title, description and canonical URL. | S |
| FR-SEO-002 | Sitemap, robots and RSS are generated. | S |
| FR-SEO-003 | Tool pages include JSON-LD `SoftwareApplication`/`WebApplication` and breadcrumb markup. | S |
| FR-SEO-004 | Open Graph and Twitter images are generated per tool. | C |
| FR-SEO-005 | An optional blog supports Markdown content, tags, RSS and per-post SEO fields. | C |
| FR-SEO-006 | Optional CMS supports draft, preview, publish and bulk publish with authentication. | C |
| FR-SEO-007 | `llms.txt` and a human-readable tool index are available. | C |

## 15. Optional modules

| Module | Description | Default | Dependencies |
|---|---|---|---|
| Content | Blog, tags, RSS, Markdown/MDX pages | Off | None for static; database for CMS |
| CMS | Authenticated admin, drafts, preview, bulk publish | Off | PostgreSQL, auth |
| Contact | Contact form with transactional email | Off | SMTP or email API |
| Analytics | Privacy-first page and event analytics | Off | Self-hosted or external endpoint |
| Accounts | Optional sync of favourites/preferences | Off | PostgreSQL, auth |
| Server processing | Fallback for jobs beyond browser limits | Off | Worker service, object storage |

Optional modules must not be imported into the core bundle and must fail closed when misconfigured.

## 16. Success metrics

Metrics are collected only with consent or from self-hosted aggregate reporting explicitly enabled by the operator.

### 16.1 Product metrics

| Metric | Definition | 1.0 target | 1.2 target |
|---|---|---|---|
| Catalogue coverage | Implemented canonical outcomes / 199 | 16.6% (33) | 100% |
| Tool success rate | Successful runs / started runs | ≥ 99% | ≥ 99% |
| Search success | Searches ending in a tool open / searches | ≥ 70% | ≥ 80% |
| Time to tool | Median time from home to first input | ≤ 15 s | ≤ 10 s |
| Repeat usage | Users returning within 30 days | ≥ 25% | ≥ 35% |
| Favourites adoption | Users with ≥ 1 favourite | ≥ 20% | ≥ 25% |
| Offline success | Offline P0 tool loads / attempts | ≥ 99% | ≥ 99% |
| Crash-free sessions | Sessions without fatal error | ≥ 99.5% | ≥ 99.7% |

### 16.2 Quality metrics

| Metric | Target |
|---|---|
| P0/P1 automated test coverage | ≥ 80% statements for tool logic |
| Accessibility violations (axe, critical/serious) | 0 |
| Bundle budget violations | 0 |
| High/critical dependency vulnerabilities | 0 at release |
| Translation coverage for launch locales | ≥ 95% |
| Documentation coverage per tool | 100% |

### 16.3 Operational metrics

| Metric | Target |
|---|---|
| Cold container start | ≤ 3 s |
| Image size (core, compressed) | ≤ 150 MB |
| Health endpoint availability | ≥ 99.9% |
| Median memory (idle) | ≤ 150 MB |
| Deployment time | ≤ 5 minutes |

## 17. Release plan and milestones

Dates are relative to project start; durations assume a small cross-functional team.

| Milestone | Scope | Duration | Exit criteria |
|---|---|---|---|
| M0 — Foundation | Repository, design system, registry, routing, search, CI, container, docs | 3 weeks | Home, category, search and one reference tool work end to end |
| M1 — Core 1.0 | 33 P0 outcomes, favourites, recents, themes, PWA, i18n scaffold, SEO basics | 8 weeks | All P0 tools pass definition of done; accessibility and performance gates pass |
| M2 — Parity 1.1 | 56 P1 outcomes, PDF and image depth, examples, locale expansion | 12 weeks | 89/199 outcomes; two-reference coverage complete |
| M3 — Long tail 1.2 | 110 P2 outcomes, media/audio/video, OCR, niche tools | 16 weeks | 199/199 outcomes; full parity statement verified |
| M4 — Optional modules | Blog, CMS, contact, accounts, analytics | 6 weeks (parallel) | Modules independently enableable; core unaffected |

### 17.1 Release gates

A release is blocked unless:

1. All scoped tools meet the definition of done.
2. Unit, integration and E2E smoke tests pass.
3. Accessibility audit passes for scoped pages.
4. Bundle and performance budgets pass.
5. Security and licence audits pass.
6. Documentation and translations meet thresholds.
7. Offline behaviour is verified for all P0 tools.
8. No reference product name, asset or copied text is present.

## 18. Definition of done for a tool

A tool is complete only when all of the following are true:

- [ ] Canonical ID and stable slug are registered.
- [ ] Title, summary, category, keywords, aliases, audience and phase are present.
- [ ] At least one original worked example exists.
- [ ] Input validation covers empty, malformed, boundary and oversized cases.
- [ ] Options have sensible defaults and a reset action.
- [ ] Output can be copied and, where applicable, downloaded.
- [ ] Errors are specific, actionable and non-destructive.
- [ ] Long jobs show progress and can be cancelled.
- [ ] File tools enforce limits and revoke object URLs.
- [ ] No user content leaves the device.
- [ ] Keyboard, screen-reader and mobile paths are verified.
- [ ] Unit tests cover happy path, edges and failure modes.
- [ ] At least one E2E smoke test covers the primary path.
- [ ] Documentation states limitations and privacy behaviour.
- [ ] Strings are externalised and translated or fallback-safe.
- [ ] Bundle impact is within budget and lazy-loaded where heavy.
- [ ] Security review is complete for cryptographic, parsing or file-processing code.
- [ ] No copied code, copy, assets or translations are present.

## 19. Risks and mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| 199-tool scope overruns | High | High | Phased releases; registry-driven delivery; P0 gate before breadth |
| Media processing exhausts memory | High | High | Workers, chunking, size limits, progress/cancel, clear guidance |
| PDF fidelity and encryption edge cases | Medium | High | Conformance fixtures; permissive libraries; explicit limitations |
| Format conversion semantic loss | Medium | Medium | Round-trip tests; warnings; documented lossy paths |
| GPL contamination | Medium | High | Clean-room policy; dependency licence audit; no code reuse |
| Translation drift | High | Medium | Typed keys, coverage CI, English fallback, beta labelling |
| Offline cache serves stale/broken builds | Medium | Medium | Versioned caches, update prompt, skip-waiting strategy |
| Search quality degrades with catalogue growth | Medium | Medium | Ranked index, aliases, synonyms, relevance tests |
| Accessibility regressions | Medium | Medium | Component primitives, axe CI, manual audits |
| Security issue in file parsers | Medium | High | Dependency pinning, sandboxing, fuzzing, disclosure process |
| Optional backend expands attack surface | Medium | High | Off by default, isolated deployment, least privilege |
| Contributor inconsistency | High | Medium | Tool template, schema, definition of done, review checklist |
| Browser codec differences | High | Medium | Feature detection, supported-format messaging, fallbacks |
| Media codec licensing | Medium | High | Use an LGPL-compatible FFmpeg build in an isolated WASM module; publish notices; keep a WebCodecs-only fallback |
| Name or brand conflict | Low | Medium | Trademark search before public launch; working name is provisional |

## 20. Open questions

| ID | Question | Owner | Needed by |
|---|---|---|---|
| OQ-01 | Final product name, domain and trademark clearance | Product | M0 |
| OQ-02 | Licence choice: MIT vs Apache-2.0 | Legal/engineering | M0 |
| OQ-03 | Whether optional accounts/sync belong in 1.x | Product | M1 |
| OQ-04 | Default analytics posture and self-host reporting | Product/privacy | M1 |
| OQ-05 | Media limits by device class and file type | Engineering | M2 |
| OQ-06 | OCR model bundling and download strategy | Engineering | M3 |
| OQ-07 | PDF compression engine selection, licensing and distribution | Legal/engineering | M2 |
| OQ-08 | Supported locale list for 1.0 | Product | M1 |
| OQ-09 | Whether server-side fallback processing is ever offered | Architecture | M2 |
| OQ-10 | Content/CMS scope and database requirement | Product | M4 |
| OQ-11 | Minimum browser versions and polyfill policy | Engineering | M0 |
| OQ-12 | Accessibility conformance evidence and audit vendor | Product | M1 |

## 21. Acceptance and sign-off

### 21.1 Product acceptance for 1.0

1. All 33 P0 outcomes are implemented and pass the definition of done.
2. A new user can find and complete a task from the home page without documentation.
3. All P0 tools work offline after first load.
4. No core tool makes a content-bearing network request.
5. The application passes accessibility, performance and security gates.
6. A self-hoster can deploy with one command and configure the instance.
7. Documentation includes competitive analysis, tool map, PRD, technical specification and operator guide.
8. No reference product names, copy, assets or code are present.

### 21.2 Sign-off

| Role | Responsibility | Name | Date |
|---|---|---|---|
| Product owner | Scope, priorities, acceptance |  |  |
| Engineering lead | Feasibility, architecture, delivery |  |  |
| Design lead | UX, accessibility, design system |  |  |
| Security reviewer | Privacy, cryptography, dependencies |  |  |
| Operations | Deployment, observability, support |  |  |

## Appendix A — P0 outcome list (33)

| Category | Outcomes |
|---|---|
| Text & strings | Case conversion; find and replace; remove duplicate lines; text statistics; text diff; slugify; Markdown to HTML |
| Data formats | JSON format/validate; JSON minify; JSON diff; JSON to CSV; CSV/TSV to JSON; XML format |
| Encoding & security | Base64 text; URL encode/decode; hash generator; UUID; password generator; JWT parser; QR code |
| Web & developer | Regex tester; cron parser/builder |
| Date & time | Unix timestamp converter |
| Numbers & math | Percentage calculator; math expression evaluator |
| Design | Colour converter |
| Images | Resize; compress; crop; JPG to PNG |
| PDF & documents | Merge; split; compress |

## Appendix B — Traceability

| Document | Role |
|---|---|
| [Competitive Analysis](./competitive-analysis.md) | Evidence base, reference profiles, platform matrix |
| [Tool Equivalence Map](./tool-map.md) | Authoritative 199-outcome catalogue with IDs, presence and phase |
| [Technical Specification](./technical-spec.md) | Architecture, contracts, pipelines and delivery design |
| [`docs/tool-map.data.json`](./tool-map.data.json) | Machine-readable catalogue generated from the matrix |

Every requirement in this document is traceable to a canonical tool ID or to a platform capability in the competitive analysis. Tests, analytics events and documentation pages must use the same IDs.
