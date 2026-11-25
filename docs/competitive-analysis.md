# Competitive Analysis

**Project:** OmniKit (working name)  
**Version:** 1.0  
**Status:** Draft for review  
**Date:** 2026-10-18

## 1. Purpose and scope

This document inventories the features of three established, self-hostable, browser-based utility suites. It establishes the evidence base for the OmniKit product requirements and the canonical [Tool Equivalence Map](./tool-map.md).

The three products are anonymised as **Reference A**, **Reference B** and **Reference C**. Their names, repository addresses and brand assets are intentionally omitted. The analysis is based on their public source trees, manifests, route tables, localisation files and documentation as observed on 2026-10-18.

Nothing in this document is copied from the references. Tool names below are neutral descriptions of user outcomes. Where a reference uses a distinctive product term, this document describes the function instead.

## 2. Method

For each reference we captured:

1. **Product shape** — rendering model, deployment, backend dependencies and target audience.
2. **Platform features** — discovery, navigation, personalisation, theming, offline support, internationalisation, analytics and content.
3. **Tool inventory** — every registered tool or tool operation, grouped by the reference's own information architecture.
4. **Delivery and quality posture** — test frameworks, container support and licence.
5. **Strengths and limitations** — what the product optimises for and where it leaves gaps.

Every tool was then normalised to a single user outcome. The normalisation rules, the 199-row canonical catalogue and the full presence matrix are in [Tool Equivalence Map](./tool-map.md). A validation script confirmed that all 86 + 133 + 14 top-level tools are represented at least once.

## 3. Reference A — developer-focused, offline-first single-page application

### 3.1 Product profile

| Attribute | Finding |
|---|---|
| Positioning | Broad developer and web-professional toolbox with a strong offline story |
| Architecture | Client-side Vue single-page application; no server runtime required |
| Stack | Vue 3, TypeScript, Vite, Naive UI, Pinia, VueUse, Vue I18n |
| Tool count | 86 tools |
| Categories | 10 |
| Locales | 9 (German, English, Spanish, French, Norwegian, Portuguese, Ukrainian, Vietnamese, Chinese) |
| Persistence | Per-tool state in browser local storage |
| Backend | None; no accounts, database or server-side processing |
| Testing | Vitest unit tests and Playwright end-to-end tests |
| Distribution | Static bundle served by nginx; container image provided |
| Licence | Strong copyleft (GPL-3.0) |

### 3.2 Platform features

- **Discovery.** Category-grouped tool directory, a "newest tools" section, and a favourites area. There is no global search.
- **Personalisation.** Favourites can be added and reordered by drag and drop. The preference persists locally.
- **Tool experience.** Each tool has a dedicated route, explanatory copy, input controls, a result area and copy-to-clipboard actions. Tool state survives reloads.
- **Theming.** Light and dark themes.
- **Offline.** A service worker generated at build time precaches the application shell and tool chunks; the installed progressive web app works offline after first load.
- **Internationalisation.** Nine complete locale files covering interface strings and tool metadata.
- **Operations.** Static hosting only. No environment variables, migrations or external services.
- **Analytics.** Optional privacy-oriented page-view analytics.
- **Content.** No blog, CMS, accounts or server-rendered marketing pages.

### 3.3 Tool inventory (86)

Reference A organises its catalogue into ten categories with the following distribution: Crypto 11, Converter 20, Web 16, Images and videos 4, Development 14, Network 6, Math 3, Measurement 3, Text 7, Data 2. The functional inventory below groups the same 86 tools for readability, not by the reference's category boundaries.

**Text and string tools (16):** ASCII text drawer; case converter; lorem ipsum generator; numeronym generator; slugify; text diff; text statistics; text to binary; text to NATO alphabet; text to Unicode; HTML entities; HTML WYSIWYG editor; Markdown to HTML; string obfuscator; emoji picker; email normalizer.

**Data and conversion tools (22):** JSON viewer; JSON minify; JSON diff; JSON to CSV; JSON to XML; JSON to YAML; JSON to TOML; TOML to JSON; TOML to YAML; YAML to JSON; YAML to TOML; YAML viewer; XML formatter; XML to JSON; list converter; date-time converter; integer base converter; roman numeral converter; temperature converter; percentage calculator; math evaluator; ETA calculator.

**Encoding, crypto and identity tools (19):** base64 string converter; base64 file converter; URL encoder; URL parser; hash text; HMAC generator; encryption/decryption; bcrypt; RSA key pair generator; BIP39 mnemonic generator; random token generator; ULID generator; UUID generator; password strength analyser; OTP generator and validator; JWT parser; basic auth header generator; QR code generator; WiFi QR code generator.

**Web and developer tools (16):** HTTP status codes; MIME types; meta tag generator; keycode information; user-agent parser; safelink decoder; git cheatsheet; regex memo; regex tester; crontab generator; SQL prettify; chmod calculator; Docker run to Docker Compose converter; benchmark builder; device information; random port generator.

**Network tools (6):** IPv4 subnet calculator; IPv4 address converter; IPv4 range expander; IPv6 ULA generator; MAC address generator; MAC address lookup.

**Other utilities (7):** camera recorder; chronometer; colour converter; IBAN validator and parser; PDF signature checker; phone parser and formatter; SVG placeholder generator.

### 3.4 Strengths

- Deep developer coverage: network maths, cryptography, regex, cron, SQL, Docker and HTTP reference data.
- Best-in-class offline behaviour among the three references.
- Mature favourites workflow and persistent per-tool state.
- Broad locale coverage and a clean static deployment model.
- Large catalogue without a backend, which keeps operations simple and privacy strong.

### 3.5 Limitations

- No global search; discovery depends on browsing categories.
- No image, audio or video editing beyond four peripheral tools.
- No PDF manipulation suite; only signature inspection.
- No server rendering, blog, CMS or account system.
- Strong copyleft licence constrains direct reuse in permissively licensed derivatives.

## 4. Reference B — everyday-task and media-focused single-page application

### 4.1 Product profile

| Attribute | Finding |
|---|---|
| Positioning | Consumer-friendly toolbox weighted toward strings, media and documents |
| Architecture | Client-side React single-page application |
| Stack | React 19, TypeScript, Vite, MUI, MUI icons, i18next, React Router |
| Tool count | 133 tools |
| Category types | 14 |
| Locales | 11 (German, English, Spanish, French, Hindi, Japanese, Dutch, Portuguese, Russian, Ukrainian, Chinese), with partial coverage |
| Persistence | User-type preference and per-tool state in local storage |
| Backend | None; files processed in the browser |
| Testing | Vitest unit tests and Playwright end-to-end tests |
| Distribution | Static bundle; small container image |
| Licence | Permissive (MIT) |

### 4.2 Platform features

- **Discovery.** Fuzzy search using Levenshtein-style matching, category cards, a "see all" view and a general-user/developer audience filter persisted locally.
- **Tool experience.** Dedicated routes, breadcrumbs, descriptive tool pages, worked examples, option panels, dedicated input and result components, and per-tool information sections.
- **Theming.** Light and dark themes.
- **Offline.** A web app manifest is present, but there is no service worker and therefore no reliable offline mode.
- **Internationalisation.** Eleven locales with incomplete translation coverage in several languages.
- **Operations.** Static hosting; no database, accounts or server processing.
- **Content.** No blog or CMS. No account system.
- **Media pipeline.** Browser APIs and client-side libraries handle image, audio, video, GIF and PDF work; no server upload is required.

### 4.3 Tool inventory (133)

Reference B groups tools into fourteen category types: image-generic, PDF, string, video, time, audio, JSON, list, CSV, number, PNG, XML, GIF and converters.

**String tools (30):** base64; bcrypt; censor; create palindrome; email extractor; extract substring; hidden character detector; join; palindrome check; password generator; quote; randomize case; remove duplicate lines; repeat; text replacer; reverse; ROT13; rotate; slug generator; split; statistic; table-text converter; text compare; to Morse; truncate; Unicode; uppercase; URL decode; URL editor; URL encode.

**Image tools (15):** change colours; change opacity; compress; create transparent PNG; crop; image editor; image to text (OCR); QR code; remove background; resize; rotate; split; watermark; compress PNG; convert JPG to PNG. Related converters add image to JPG and image to WebP.

**Number tools (12):** arithmetic sequence; byte converter; number sequence generator; area of sphere; cable voltage drop; Ohm's law; slackline tension; volume of sphere; number to words; random number generator; random port generator; sum numbers.

**Time tools (12):** leap year checker; days to hours; hours to days; seconds to time; time to decimal; time to seconds; Unix to date; crontab guru; date weekday calculator; Discord timestamp; time between dates; truncate clock time.

**Video tools (12):** add audio; change speed; compress; crop; flip; GIF change speed; images to GIF; loop; merge; resize; rotate; trim.

**CSV tools (11):** change separator; rows to columns; CSV to JSON; CSV to TSV; CSV to XML; CSV to YAML; find incomplete records; insert columns; swap columns; transpose; TSV to JSON.

**List tools (11):** group; duplicate; most popular; unique; reverse; rotate; shuffle; sort; truncate; unwrap; wrap. A chunk implementation exists in the codebase but is not registered as a tool.

**PDF tools (10):** compress; images to PDF; editor; extract images; merge; PDF to EPUB; PDF to PNG; protect; rotate; split.

**JSON tools (9):** escape; comparison; JSON to CSV; JSON to XML; minify; prettify; sort; stringify; validate.

**Audio tools (5):** change speed; extract audio; loop; merge; trim.

**Converter tools (4):** audio converter; convert to JPG; convert to WebP; video to GIF.

**XML tools (2):** beautifier; validator.

### 4.4 Strengths

- Largest catalogue of the three, with strong coverage of everyday text, list, CSV, time and number tasks.
- The deepest media suite: image editing, audio editing, video editing and GIF creation all run client-side.
- Good discovery through fuzzy search, audience filtering, category cards and examples.
- Permissive licence and a lean static deployment.
- A practical PDF suite covering the most common document operations.

### 4.5 Limitations

- No favourites, recent-tools list or cross-device personalisation.
- No service worker, so the PWA manifest does not deliver offline use.
- Translation coverage is uneven across the eleven locales.
- No server rendering, SEO content, blog, CMS or accounts.
- Media processing is constrained by browser memory and codec support; very large files can fail without clear recovery paths.
- Some catalogue inconsistencies exist, such as an implemented but unregistered list-chunking tool.

## 5. Reference C — server-rendered, content- and document-focused application

### 5.1 Product profile

| Attribute | Finding |
|---|---|
| Positioning | Search-friendly utility site with a document suite and an editorial backend |
| Architecture | Next.js server-rendered application with client-side tool execution |
| Stack | Next.js 16, React 19, TypeScript, Tailwind CSS, next-intl |
| Tool count | 14 top-level tools; 19 operations when PDF operations are counted separately |
| Categories | 6 |
| Locales | 2 (Italian default, English) |
| Persistence | No tool-state persistence of note |
| Backend | PostgreSQL with Prisma, JWT authentication, admin CMS, contact delivery, revalidation and health endpoints |
| Testing | Jest unit tests and Playwright end-to-end tests |
| Distribution | Container and Kubernetes deployment |
| Licence | Permissive (MIT) |

### 5.2 Platform features

- **Discovery.** Global tool search, category navigation and dedicated tool pages.
- **Tool experience.** Server-rendered landing and tool pages with explanatory content, structured data and FAQ blocks. Tools execute in the browser.
- **Theming.** Dark interface only; no light-theme toggle.
- **Offline.** Web app manifest only; no service worker.
- **Internationalisation.** Two locales with server-side locale routing.
- **Content.** Blog with a PostgreSQL-backed CMS and admin area: posts, tags, previews, bulk publishing and SEO fields; Markdown and rich-text editing.
- **SEO.** Server-side rendering, metadata, sitemap, robots, RSS, `llms.txt`, dynamic Open Graph images, JSON-LD structured data and FAQ markup.
- **Engagement.** Contact form with transactional email delivery and cookie consent.
- **Analytics.** Google Analytics and Search Console integration.
- **Operations.** Docker and Kubernetes assets, health endpoints and on-demand revalidation.
- **Document suite.** Merge, split, compress, edit, images-to-PDF and PDF-to-images. Compression uses a WebAssembly-based PDF pipeline.

### 5.3 Tool inventory (14 top-level, 19 operations)

**Text Processing (3):** JSON formatter; Markdown converter; lorem ipsum generator.

**Encoding (2):** base64 encoder/decoder; URL encoder/decoder.

**Development (6):** regex tester; diff checker; cron builder; JSON Schema generator; XML/WSDL viewer; UUID generator.

**Security (1):** hash generator.

**Design (1):** colour converter.

**Documents (1 tool, 6 operations):** PDF merge; PDF split; PDF compress; PDF editor with page rotate, delete, reorder and preview; images to PDF with page-size and quality controls; PDF to images. The tool group is presented as a single document workspace.

### 5.4 Strengths

- Strongest discovery and acquisition posture: server rendering, metadata, structured data, sitemap, RSS and dynamic social images.
- Only reference with a real backend: CMS, admin workflows and contact handling.
- Most complete PDF suite, including genuine compression and page-level editing.
- Clean, focused user experience with global search.
- Permissive licence and production-oriented deployment assets.

### 5.5 Limitations

- Smallest tool catalogue by an order of magnitude.
- No favourites, no light theme and no offline mode.
- Backend and database add operational cost and attack surface for what is mostly a static utility product.
- Only two locales.
- Tool state is not preserved between sessions.
- Media editing beyond PDF is absent.

## 6. Platform feature matrix

| Capability | Reference A | Reference B | Reference C | OmniKit target |
|---|---|---|---|---|
| Rendering | Client SPA | Client SPA | Server-rendered Next.js | Static-first with optional server rendering |
| Tool count | 86 | 133 | 14 top-level (19 operations) | 199 canonical outcomes |
| Categories | 10 | 14 | 6 | 13 canonical categories |
| Global search | No | Yes, fuzzy | Yes | Yes, fuzzy, keyboard-first |
| Category browsing | Yes | Yes | Yes | Yes |
| Audience filter | No | Yes | No | Yes, optional |
| Favourites | Yes, reorderable | No | No | Yes, reorderable and syncable |
| Recent tools | No | No | No | Yes |
| Newest tools | Yes | No | No | Yes |
| Tool examples | Limited | Yes | Limited | Yes, for every tool |
| Per-tool state persistence | Yes | Yes | No | Yes |
| Copy results | Yes | Yes | Yes | Yes, with feedback |
| Light/dark theme | Yes | Yes | Dark only | Yes, plus system |
| PWA manifest | Yes | Yes | Yes | Yes |
| Service worker / offline | Yes | No | No | Yes |
| Internationalisation | 9 locales | 11 locales, partial | 2 locales | 8+ locales, typed keys |
| Backend required | No | No | Yes | No for core; optional for CMS |
| Accounts | No | No | Admin only | Optional; anonymous by default |
| Database | No | No | PostgreSQL | Optional |
| Blog / CMS | No | No | Yes | Optional module |
| Contact form | No | No | Yes | Optional module |
| Analytics | Privacy-oriented | None observed | Google Analytics | Pluggable, privacy-first, off by default |
| SEO metadata | Basic | Basic | Strong | Strong via pre-render |
| Sitemap / robots | No | No | Yes | Yes |
| RSS | No | No | Yes | Optional |
| Structured data | No | No | Yes | Yes |
| Dynamic OG images | No | No | Yes | Optional |
| Image editing | No | Yes | No | Yes |
| Audio editing | No | Yes | No | Yes |
| Video editing | No | Yes | No | Yes |
| PDF suite | Signature check only | Yes | Yes | Yes |
| Client-side processing | Yes | Yes | Yes | Yes, default |
| Container deployment | Yes | Yes | Yes | Yes |
| Unit and E2E tests | Yes | Yes | Yes | Yes |
| Licence | GPL-3.0 | MIT | MIT | To be decided; permissive preferred |

## 7. Same, different and missing

### 7.1 What all three do

Nine canonical outcomes appear in every reference:

1. Text diff and comparison
2. JSON formatting and validation
3. JSON minification
4. JSON diff
5. JSON to CSV
6. XML formatting
7. Base64 text encoding and decoding
8. URL encoding and decoding
9. Cron parsing and building

These are the universal baseline. A product that lacks any of them will feel incomplete to users arriving from any of the references.

### 7.2 What two of three do

- **A + B:** case conversion, text statistics, slugify, Unicode escaping, JSON to XML, URL parsing/editing, bcrypt, random port generation, QR codes, Unix timestamps.
- **A + C:** lorem ipsum, Markdown to HTML, JSON to YAML, MD5/SHA hashing, UUID generation, regex testing, colour conversion.
- **B + C:** CSV/TSV to JSON, XML validation, PDF merge, PDF split, PDF compression, PDF editing, images to PDF, PDF to images.

### 7.3 What only one does

- **Only A:** 61 outcomes, concentrated in cryptography, network maths, developer reference data, niche generators and encoding formats.
- **Only B:** 101 outcomes, concentrated in string manipulation, list processing, CSV transforms, media editing, calculators and everyday conversions.
- **Only C:** 3 outcomes: JSON tree viewing, JSON Schema generation, and WSDL/XML tree inspection with XPath.

### 7.4 Coverage summary

| Presence | Canonical outcomes | Share of 199 |
|---|---:|---:|
| All three | 9 | 4.5% |
| A + B only | 10 | 5.0% |
| A + C only | 7 | 3.5% |
| B + C only | 8 | 4.0% |
| A only | 61 | 30.7% |
| B only | 101 | 50.8% |
| C only | 3 | 1.5% |
| **Total** | **199** | **100%** |

The long tail is the story: **82.9% of the combined catalogue is unique to a single reference**. No existing product covers more than about two thirds of the union, and the largest single gap is everyday string, list and media work.

## 8. Product implications

### 8.1 Table stakes

OmniKit must match the universal nine on day one, plus the platform behaviours users now expect: fast search, category browsing, favourites, recent tools, per-tool state, copy actions, light/dark themes, mobile layout, offline support and container deployment. These are not differentiators; they are the entry ticket.

### 8.2 The parity opportunity

The references specialise:

- A is deep for developers and offline use.
- B is broad for everyday and media tasks.
- C is strong for discovery, content and documents.

OmniKit can win by combining all three emphases in one product:

1. **Breadth** — all 199 outcomes, including the 165 outcomes that only one reference offers.
2. **Depth** — upgrade every partial outcome to a first-class tool rather than an option buried in another screen.
3. **Discovery** — fuzzy search, audience filters, examples and strong pre-rendered metadata.
4. **Trust** — local-first processing, no uploads by default, clear privacy language and optional self-hosted services.
5. **Craft** — consistent tool shell, keyboard support, accessible controls, sensible defaults and recoverable errors.

### 8.3 What not to copy

- Do not copy text, screenshots, icons, illustrations, translations, sample data or source code.
- Do not inherit a strong copyleft obligation by lifting implementation from the developer-focused reference.
- Do not reproduce uneven translation coverage or unregistered/orphaned tools.
- Do not make a database mandatory for core utilities.
- Do not ship a manifest without a service worker and call the product offline-capable.
- Do not accept unbounded media jobs that crash the tab; design for memory limits and graceful failure.

### 8.4 Recommended positioning

> **OmniKit is the complete, private, self-hosted toolbox: every everyday conversion, text, data, media and document task in one fast, searchable, offline-capable application — with no uploads, no accounts and no lock-in.**

The product should be evaluated against four promises:

| Promise | Proof point |
|---|---|
| Complete | 199 canonical outcomes, all 233 source instances mapped |
| Private | Client-side processing by default; no telemetry without consent |
| Fast | Static-first delivery, lazy media chunks, sub-second search |
| Ownable | Docker Compose, no mandatory external services, permissive licence |

## 9. Risks visible in the competitive set

| Risk | Evidence | Mitigation |
|---|---|---|
| Catalogue sprawl | The largest reference has 133 tools with inconsistent registration and translation | Registry-driven catalogue, schema validation, definition of done per tool |
| Media memory exhaustion | Browser-based video and PDF work is memory hungry | Worker isolation, size limits, chunking, progress and cancellation |
| Format fidelity | Many converters exist across products with different edge-case behaviour | Shared parser/serialiser layer and conformance fixtures |
| Translation drift | One reference reports 67–96% coverage | Typed keys, coverage checks in CI, English fallback |
| Offline overpromise | Two references ship a manifest without a service worker | Test offline behaviour as a release gate |
| Security surface | The server-rendered reference carries a database, admin and contact pipeline | Keep core static; make server modules optional and isolated |
| Licence contamination | The developer-focused reference is GPL-3.0 | Clean-room implementation, dependency licence audit, no code reuse |
| Stale reference data | HTTP codes, MIME types, user agents and MAC vendors change | Version and date bundled datasets; document update cadence |

## 10. Source artefacts

| Artefact | Location | Purpose |
|---|---|---|
| Canonical tool matrix | [`docs/tool-map.md`](./tool-map.md) | 199 outcomes with presence, phase and notes |
| Machine-readable matrix | [`docs/tool-map.data.json`](./tool-map.data.json) | Generated from the canonical matrix |
| Reference inventories | Research workspace, not committed | Raw route and metadata extracts used for validation |
| Validation script | Research workspace, not committed | Proves 86/86, 133/133 and 14/14 coverage |

## 11. Conclusions

1. The combined market offers **233 tool instances** but only **199 distinct outcomes**; duplication is modest and the long tail is large.
2. Only **nine outcomes** are universal. They define the minimum viable product.
3. The biggest unmet need is a single product that is simultaneously **broad (B), deep for developers (A), and discoverable and document-capable (C)**.
4. A static-first architecture can deliver nearly everything; a backend should be an optional module for content and administration, never a dependency for tools.
5. Complete parity is a **199-tool programme**, not a single release. It should be sequenced P0 core, P1 parity, P2 long tail and media.
6. Clean-room implementation and a permissive licence are strategic requirements, not legal afterthoughts.

The requirements that follow from these conclusions are specified in the [Product Requirements Document](./PRD.md) and the [Technical Specification](./technical-spec.md).
