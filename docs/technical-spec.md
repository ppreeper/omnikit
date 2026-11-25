# Technical Specification

**Project:** OmniKit (working name)  
**Version:** 1.0  
**Status:** Draft for review  
**Date:** 2026-10-18  
**Related documents:** [PRD](./PRD.md) · [Tool Equivalence Map](./tool-map.md) · [Competitive Analysis](./competitive-analysis.md)

## 1. Overview

OmniKit is a self-hosted, local-first utility application. It serves a catalogue of 199 tool outcomes through a static-first web application, executes user work in the browser, and optionally enables server-side modules for content, contact and administration.

The architecture optimises for five properties in order:

1. **Privacy** — user content never leaves the browser for core tools.
2. **Performance** — the home page and text tools stay small; heavy engines load only when needed.
3. **Correctness** — deterministic tool logic with conformance fixtures.
4. **Operability** — one container, no mandatory services, predictable resource use.
5. **Extensibility** — a registry-driven contract makes the 200th tool routine.

## 2. Architecture summary

```
┌─────────────────────────────────────────────────────────────────────┐
│                              Browser                                │
│                                                                     │
│  ┌──────────────┐   ┌──────────────┐   ┌─────────────────────────┐  │
│  │  App shell   │   │  Tool UI     │   │  Processing layer       │  │
│  │  router,     │──▶│  input,      │──▶│  pure functions         │  │
│  │  search,     │   │  options,    │   │  Web Workers            │  │
│  │  favourites   │   │  result      │   │  WASM (PDF, media, OCR) │  │
│  └──────────────┘   └──────────────┘   └─────────────────────────┘  │
│         │                  │                       │                │
│         ▼                  ▼                       ▼                │
│  localStorage        URL / route state      Object URLs / Blobs     │
└─────────────────────────────────────────────────────────────────────┘
                                  │
                     static assets over HTTPS
                                  │
┌─────────────────────────────────────────────────────────────────────┐
│                     Delivery (one of two modes)                     │
│                                                                     │
│  Static mode (default)              Server mode (optional)          │
│  ├─ CDN / nginx / Caddy             ├─ Next.js Node runtime         │
│  ├─ immutable build assets          ├─ PostgreSQL (optional)        │
│  ├─ no runtime secrets              ├─ CMS, contact, dynamic OG     │
│  └─ all 199 tools client-side       └─ core tools still client-side │
└─────────────────────────────────────────────────────────────────────┘
```

### 2.1 Key decisions

| Decision | Choice | Rationale |
|---|---|---|
| Framework | Next.js 16 App Router, React 19, TypeScript strict | Static generation for SEO, optional server runtime, mature ecosystem |
| Styling | Tailwind CSS 4 + CSS custom properties + Radix primitives | Consistent design system, accessible primitives, small runtime |
| State | Zustand with persisted slices; TanStack Query only for optional server data | Minimal global state; tool state stays local |
| Search | MiniSearch, prebuilt index | Small, fast, fuzzy, no server |
| Validation | Zod at boundaries | Shared schemas for registry, config, storage and forms |
| i18n | next-intl with typed message keys | Locale routing, static generation, fallback |
| PWA | Serwist service worker | Precaching, versioned caches, update flow |
| Workers | Native module workers + Comlink | Typed RPC, cancellation, isolation |
| Media | FFmpeg.wasm, WebCodecs where available | Broad format support without a server |
| PDF | pdf-lib, pdfjs-dist, qpdf WASM or equivalent | Editing, rendering, licence-safe compression |
| OCR | Tesseract.js with self-hosted language data | No third-party runtime calls |
| Database (optional) | PostgreSQL 16 + Drizzle ORM | Typed schema, migrations, small footprint |
| Package manager | pnpm | Deterministic installs, workspace support |
| Runtime | Node.js 22 LTS (build and optional server) | Supported, stable, container-friendly |
| Licence | MIT or Apache-2.0 (decision pending) | Permissive, compatible with self-hosting |

### 2.2 Why not a pure SPA or a server-first app

- A pure SPA sacrifices pre-rendered metadata and per-tool SEO unless a separate prerender step is added.
- A server-first app makes a database and Node runtime mandatory for tasks that are entirely local, increasing cost and attack surface.
- Next.js with static generation gives pre-rendered pages and a client-side tool runtime, while allowing optional server modules in a separate deployment mode.

## 3. Deployment modes

### 3.1 Static mode (default)

- `next build` with static generation for all public routes.
- All 199 tools run client-side.
- No database, no runtime secrets, no Node process required.
- Served by nginx, Caddy, a CDN or any static host.
- Dynamic features degrade gracefully: contact and CMS are disabled; Open Graph images are generated at build time.

**Use when:** privacy, simplicity and low operations cost matter most.

### 3.2 Server mode (optional)

- Next.js standalone Node server.
- Core tools remain client-side.
- Enables CMS, contact form, dynamic Open Graph images, revalidation and optional accounts.
- PostgreSQL required only for CMS/accounts.
- Health and readiness endpoints exposed.

**Use when:** the operator wants editorial content, a contact form or central administration.

### 3.3 Mode selection

`OMNIKIT_MODE=static|server` controls build-time feature flags. Server-only modules are dynamically imported and excluded from the static bundle. Invalid combinations fail at startup.

## 4. Repository structure

```
omnikit/
├─ apps/
│  └─ web/
│     ├─ app/
│     │  ├─ [locale]/
│     │  │  ├─ layout.tsx
│     │  │  ├─ page.tsx
│     │  │  ├─ tools/
│     │  │  │  ├─ page.tsx
│     │  │  │  └─ [slug]/page.tsx
│     │  │  ├─ category/[slug]/page.tsx
│     │  │  ├─ favourites/page.tsx
│     │  │  ├─ recent/page.tsx
│     │  │  ├─ about/page.tsx
│     │  │  ├─ privacy/page.tsx
│     │  │  ├─ help/page.tsx
│     │  │  └─ blog/...            # optional
│     │  ├─ api/                   # server mode only
│     │  │  ├─ health/route.ts
│     │  │  ├─ contact/route.ts
│     │  │  ├─ revalidate/route.ts
│     │  │  └─ admin/...
│     │  ├─ manifest.ts
│     │  ├─ sitemap.ts
│     │  ├─ robots.ts
│     │  └─ globals.css
│     ├─ components/
│     │  ├─ shell/
│     │  ├─ discovery/
│     │  ├─ tool/
│     │  ├─ ui/
│     │  └─ content/
│     ├─ tools/
│     │  ├─ registry.ts
│     │  ├─ schema.ts
│     │  ├─ categories.ts
│     │  ├─ text/
│     │  │  ├─ case-converter/
│     │  │  │  ├─ meta.ts
│     │  │  │  ├─ tool.tsx
│     │  │  │  ├─ logic.ts
│     │  │  │  ├─ logic.test.ts
│     │  │  │  └─ fixtures/
│     │  │  └─ ...
│     │  ├─ data/
│     │  ├─ encoding/
│     │  ├─ developer/
│     │  ├─ time/
│     │  ├─ numbers/
│     │  ├─ network/
│     │  ├─ design/
│     │  ├─ lists/
│     │  ├─ images/
│     │  ├─ video/
│     │  ├─ audio/
│     │  └─ pdf/
│     ├─ lib/
│     │  ├─ storage/
│     │  ├─ search/
│     │  ├─ workers/
│     │  ├─ crypto/
│     │  ├─ files/
│     │  ├─ i18n/
│     │  ├─ analytics/
│     │  └─ utils/
│     ├─ messages/
│     │  ├─ en/
│     │  ├─ de/
│     │  └─ ...
│     ├─ public/
│     │  ├─ icons/
│     │  ├─ wasm/
│     │  ├─ ocr/
│     │  └─ og/
│     ├─ scripts/
│     │  ├─ build-search-index.ts
│     │  ├─ validate-registry.ts
│     │  ├─ check-translations.ts
│     │  └─ check-bundle-budget.ts
│     ├─ next.config.ts
│     ├─ tailwind.config.ts
│     ├─ tsconfig.json
│     └─ package.json
├─ packages/
│  ├─ tool-sdk/                    # shared types and test helpers
│  └─ config/                      # eslint, tsconfig, tailwind presets
├─ deploy/
│  ├─ docker/
│  │  ├─ Dockerfile.static
│  │  ├─ Dockerfile.server
│  │  └─ nginx.conf
│  ├─ compose/
│  │  ├─ docker-compose.yml
│  │  └─ docker-compose.server.yml
│  └─ k8s/
├─ docs/
├─ scripts/
├─ pnpm-workspace.yaml
└─ README.md
```

## 5. Tool registry contract

### 5.1 Definition

The registry is the single source of truth for routing, search, categories, favourites, sitemap, tests and documentation.

```ts
// packages/tool-sdk/src/types.ts
import type { ComponentType } from 'react';

export type ToolId = string;           // e.g. "TXT-12"
export type ToolSlug = string;         // e.g. "text-diff"
export type CategoryId =
  | 'text'
  | 'data'
  | 'encoding'
  | 'developer'
  | 'time'
  | 'numbers'
  | 'network'
  | 'design'
  | 'lists'
  | 'images'
  | 'video'
  | 'audio'
  | 'pdf';

export type Phase = 'P0' | 'P1' | 'P2';
export type Audience = 'general' | 'developer';

export interface ToolExample {
  nameKey: string;
  input: unknown;
  options?: Record<string, unknown>;
  expected: unknown;
}

export interface ToolCapabilities {
  files: boolean;
  network: false;             // core tools must declare false
  randomness: boolean;
  wasm: boolean;
  worker: boolean;
  ocr?: boolean;
  media?: boolean;
}

export interface ToolLimits {
  maxFileBytes?: number;
  maxFiles?: number;
  maxTextBytes?: number;
  maxItems?: number;
  maxPages?: number;
  maxDurationSeconds?: number;
}

export interface ToolDefinition {
  id: ToolId;
  slug: ToolSlug;
  category: CategoryId;
  phase: Phase;
  audience: Audience[];
  titleKey: string;
  summaryKey: string;
  descriptionKey?: string;
  keywords: string[];
  aliases?: string[];
  icon: string;
  related?: ToolSlug[];
  capabilities: ToolCapabilities;
  limits?: ToolLimits;
  examples: ToolExample[];
  load: () => Promise<{ default: ComponentType }>;
  worker?: () => Promise<Worker>;
  seo?: {
    titleKey?: string;
    descriptionKey?: string;
    faq?: Array<{ questionKey: string; answerKey: string }>;
  };
}
```

### 5.2 Registry rules

- IDs are immutable and match the canonical matrix.
- Slugs are immutable after release and never reused.
- Metadata files must not import tool implementations or heavy libraries.
- `load` must use a dynamic import so the tool is a separate chunk.
- `capabilities.network` must be `false` for all core tools; the schema rejects `true` in static mode.
- `examples` must include at least one valid case and one failure case is recommended.
- Related tools are symmetric where practical; a build check warns on one-way links.

### 5.3 Validation

`scripts/validate-registry.ts` checks:

1. Zod schema conformance.
2. Unique IDs and slugs.
3. Every canonical ID from `docs/tool-map.data.json` is present exactly once.
4. Every category has at least one tool.
5. Every tool has an example, keywords and translations for the source locale.
6. Every `load` resolves at build time.
7. No metadata file imports a component or library over the metadata budget.
8. Related links resolve.
9. Phase totals match the catalogue (33/56/110).
10. No forbidden reference names appear anywhere in the repository.

## 6. Routing and rendering

### 6.1 Route generation

- `generateStaticParams` emits every locale and tool slug.
- Tool pages are statically generated from metadata; the interactive component hydrates on the client.
- Unknown slugs call `notFound()`.
- Canonical URLs exclude trailing slashes and duplicate locale prefixes.

### 6.2 Rendering strategy

| Route | Strategy | Notes |
|---|---|---|
| Home | Static + client search | Search index loaded after first paint |
| Category | Static | Generated per category and locale |
| Tool | Static shell + client tool | Tool chunk lazy-loaded |
| Favourites / recent | Client-only | Local storage; static shell |
| About / privacy / help | Static | MDX or TSX |
| Blog | Static (default) or server (CMS) | Revalidation in server mode |
| Admin | Server-only | Dynamic, authenticated |
| Health | Server-only | No cache |

### 6.3 Metadata

Each tool page exports `generateMetadata` using registry metadata and translations. Required fields: title, description, canonical, `hreflang`, Open Graph title/description/image, Twitter card. JSON-LD includes `WebApplication`, `BreadcrumbList` and optional `FAQPage`.

## 7. Tool runtime

### 7.1 Layers

```
meta.ts        metadata only, no runtime imports
tool.tsx       UI composition, local state, calls logic
logic.ts       pure functions, no DOM, unit-testable
worker.ts      optional worker entry for heavy work
fixtures/      canonical inputs and expected outputs
```

### 7.2 Execution paths

| Workload | Path | Threshold |
|---|---|---|
| Short text, small JSON, small lists | Main thread, pure function | < 50 ms expected |
| Large text, CSV, JSON, diff | Worker | > 1 MB or > 10k lines |
| Hashing, bcrypt, encryption | Worker | Always for bcrypt; WebCrypto for hashes |
| Image decode/encode | Worker + OffscreenCanvas | Always for files > 4 MB |
| PDF parse/render/merge | Worker + WASM | Always |
| Video/audio | Worker + FFmpeg.wasm | Always |
| OCR | Worker + Tesseract.js | Always |

### 7.3 Async state machine

Every tool UI uses a shared state machine:

```
idle → validating → running → success
                  ↘ error
                  ↘ cancelled
```

Rules:

- Validation is synchronous where possible and runs before allocation.
- `running` shows progress after 300 ms.
- Cancellation sets `cancelled`, terminates the worker if necessary and releases object URLs.
- Errors include a stable code, a user message key and optional detail.
- Success never mutates input.

### 7.4 Worker protocol

```ts
// request
type WorkerRequest =
  | { id: string; type: 'run'; payload: unknown; options: unknown }
  | { id: string; type: 'cancel' };

// response
type WorkerResponse =
  | { id: string; type: 'progress'; value: number; label?: string }
  | { id: string; type: 'result'; value: unknown }
  | { id: string; type: 'error'; code: string; message: string };
```

- Each job carries a unique ID.
- The client enforces a timeout per tool class.
- Workers are terminated on cancellation, route change or memory pressure.
- Comlink wraps the protocol for type safety; raw `postMessage` is allowed for streaming.

### 7.5 Cancellation and cleanup

- `AbortController` is passed to fetch-free local pipelines as a cooperative token.
- Object URLs are tracked per tool session and revoked on reset/unmount.
- Blobs and large buffers are released by dropping references and terminating workers.
- Service worker caches never store user files.

## 8. Tool UI contract

### 8.1 Shared components

| Component | Responsibility |
|---|---|
| `ToolPage` | Layout, breadcrumb, metadata, related tools |
| `ToolHeader` | Title, summary, privacy indicator, phase badge |
| `ToolExample` | Example selector and load action |
| `ToolForm` | Submit handling, validation summary, keyboard behaviour |
| `ToolInput` | Text, textarea, number, select, toggle, file drop |
| `ToolOptions` | Collapsible options with reset |
| `ToolActions` | Primary action, cancel, copy, download, reset |
| `ToolResult` | Result rendering, metadata, empty/error states |
| `ToolError` | Stable code, message, recovery action |
| `ToolLimits` | Documented limits and privacy note |
| `RelatedTools` | Curated links |

### 8.2 Rules

- No tool implements its own button, input, dialog or toast styles.
- All controls use the design-system primitives.
- All async status changes are announced through a polite live region.
- Primary action is reachable with `Enter`/`Ctrl+Enter` as appropriate.
- File drop zones are also clickable and keyboard-operable.
- Result areas never auto-scroll away from the input on mobile.
- Destructive resets confirm only when data would be lost.

### 8.3 Error taxonomy

| Code | Meaning | Recovery |
|---|---|---|
| `INPUT_EMPTY` | Required input missing | Focus field |
| `INPUT_INVALID` | Parse/format failure | Show line/column |
| `INPUT_TOO_LARGE` | Limit exceeded | Show limit and guidance |
| `UNSUPPORTED_FORMAT` | Codec/type unsupported | List supported types |
| `PROCESSING_FAILED` | Engine failure | Retry or simplify |
| `OUT_OF_MEMORY` | Allocation failure | Reduce size, close tabs |
| `CANCELLED` | User cancelled | Return to idle |
| `TIMEOUT` | Job exceeded budget | Retry with smaller input |
| `NETWORK_DISABLED` | Tool attempted network | Bug; report |

## 9. State and persistence

### 9.1 Storage keys

| Key | Contents | Retention |
|---|---|---|
| `omnikit.v1.theme` | `light` / `dark` / `system` | Until cleared |
| `omnikit.v1.locale` | Locale code | Until cleared |
| `omnikit.v1.favourites` | Ordered slug array | Until cleared |
| `omnikit.v1.recent` | Last 12 slugs with timestamps | Rolling |
| `omnikit.v1.audience` | `all` / `general` / `developer` | Until cleared |
| `omnikit.v1.tool.<slug>` | Tool-specific safe state | Until cleared |
| `omnikit.v1.version` | Storage schema version | Replaced on migration |

### 9.2 Rules

- All persisted values are validated with Zod on read.
- Corrupt values are discarded and logged locally without content.
- Storage is versioned; migrations run once and are idempotent.
- Sensitive fields opt out of persistence via `persist: false`.
- JWT contents, passwords, private keys, mnemonics and file data are never persisted.
- A "clear local data" action removes all `omnikit.v1.*` keys.
- Storage quota failures degrade to in-memory state with a notice.

### 9.3 Cross-tab behaviour

- Theme, locale and favourites synchronise through the `storage` event.
- Tool state is tab-local to avoid overwriting active work.
- Updates use last-write-wins with a timestamp; conflicts are rare and non-destructive.

## 10. Search and discovery

### 10.1 Index

MiniSearch index fields:

| Field | Boost | Notes |
|---|---:|---|
| `title` | 4 | Exact and prefix matches rank highest |
| `aliases` | 3 | Common alternate names |
| `keywords` | 2 | Synonyms and technical terms |
| `summary` | 1 | Natural-language description |
| `category` | 1 | Category name is searchable |

Options: prefix search, fuzzy 0.2, combine with OR, tokenize on whitespace and punctuation.

### 10.2 Build and load

- `scripts/build-search-index.ts` generates `public/search-index.json` at build.
- The index is fetched on home and all-tools pages, cached by the service worker.
- If the fetch fails, search falls back to a simple in-memory title/keyword filter.
- Index size budget: ≤ 150 KB uncompressed for 199 tools.

### 10.3 Ranking and UX

- Exact title match first, then title prefix, alias, keyword, summary, fuzzy.
- Category filter and audience filter compose with the query.
- Keyboard: arrow keys move, `Enter` opens, `Escape` clears.
- Empty query shows favourites, recents and categories.
- Zero results show suggestions and a "browse all" action.
- Search analytics are opt-in and store only the result count and selected slug, never the query text.

## 11. Internationalisation

### 11.1 Structure

```
messages/
├─ en/
│  ├─ common.json
│  ├─ categories.json
│  ├─ tools.json
│  ├─ errors.json
│  └─ seo.json
├─ de/
└─ ...
```

- `next-intl` provides locale routing and message loading.
- Message keys are typed through a generated union.
- Tool metadata stores keys, not strings.
- Missing keys fall back to English and are reported by CI.
- Locale detection order: URL prefix, stored preference, `Accept-Language`, default.

### 11.2 Formatting

- Dates and times use `Intl.DateTimeFormat`.
- Numbers use `Intl.NumberFormat`.
- File sizes use locale-aware units with IEC/SI labelling.
- Lists use `Intl.ListFormat` where appropriate.
- Text direction is set on `<html dir>`; layout uses logical CSS properties.

### 11.3 Coverage gates

- Source locale must be 100%.
- Launch locales must be ≥ 95% for P0/P1 strings.
- A locale below 80% is labelled beta and falls back per key.
- Unused keys fail CI after a grace period.

## 12. Theming and design system

- CSS custom properties define colour, spacing, radius, typography, elevation and motion.
- Light, dark and system themes; no flash by applying the theme class before paint via a small inline script or server cookie.
- Tokens meet WCAG AA contrast in both themes.
- Components use Radix primitives for dialog, popover, tabs, tooltip, select and toast.
- Icons are a permissively licensed set or original SVGs, tree-shaken per tool.
- Motion uses duration/easing tokens and respects `prefers-reduced-motion`.
- No runtime CSS-in-JS; Tailwind utilities plus component classes.

## 13. Accessibility implementation

| Area | Implementation |
|---|---|
| Landmarks | `header`, `nav`, `main`, `footer`; skip link first in tab order |
| Focus | Managed on route change and dialog open/close; visible ring token |
| Forms | Explicit labels, help text, `aria-describedby`, inline errors |
| Live regions | Polite status for progress and copy; assertive for errors |
| Keyboard | All actions reachable; drag-and-drop has move up/down buttons |
| Media | Captions/transcripts for instructional content; no autoplay |
| Colour | Contrast tokens; patterns/labels supplement colour |
| Zoom | Usable at 200% and 400% reflow |
| Motion | Reduced-motion variants |
| Testing | axe in CI, keyboard scripts, manual screen-reader passes |

## 14. Processing pipelines

### 14.1 Text and strings

- Pure TypeScript functions; no DOM dependencies.
- Grapheme-aware operations use `Intl.Segmenter` where supported with a fallback.
- Line endings are preserved unless normalisation is requested.
- Regex tools run in a worker with input and time caps; catastrophic backtracking is terminated.
- Markdown rendering uses markdown-it with HTML disabled by default, then DOMPurify.
- WYSIWYG output is sanitised on render and export.

### 14.2 Data formats

| Format | Library | Notes |
|---|---|---|
| JSON | Native `JSON` + custom error locator | Preserve key order; big integers handled as strings with warning |
| YAML | `js-yaml` | Safe schema only; no custom tags |
| TOML | `smol-toml` | Spec-compliant parse/stringify |
| XML | `fast-xml-parser` | Namespace-aware; preserve attributes with prefixes |
| CSV/TSV | `papaparse` | Quoted fields, embedded newlines, delimiter detection |
| JSON Schema | Custom generator | Draft 2020-12 and Draft-07 output |

Rules:

- Parse errors report line and column.
- Conversions that lose type information warn before running.
- Large documents stream or chunk in a worker.
- Round-trip fixtures cover representative structures, Unicode, nulls, numbers and nested arrays.

### 14.3 Encoding and cryptography

| Function | Implementation |
|---|---|
| Base64 | `Uint8Array` + native `btoa`/`atob` with chunking |
| URL | `encodeURIComponent` / `URLSearchParams` |
| Hashes | WebCrypto for SHA; `hash-wasm` for MD5/SHA-3 where needed |
| HMAC | WebCrypto |
| AES | WebCrypto AES-GCM/CBC with explicit IV handling |
| TripleDES/Rabbit | `crypto-js`, lazy-loaded, marked legacy |
| bcrypt | `bcryptjs` in a worker |
| RSA | WebCrypto key generation/import/export |
| BIP39 | `@scure/bip39` |
| UUID | `crypto.randomUUID` plus v1/v7 generators |
| ULID | `ulid` |
| OTP | `otpauth` |
| QR | `qrcode` |
| JWT | Base64url decode only; explicit "not verified" warning |

Security rules:

- Never log secrets, keys, passwords or plaintext.
- Clear sensitive fields on reset and route change.
- Use constant-time comparison where applicable.
- Label legacy algorithms and discourage production use.
- Do not implement cryptography primitives from scratch.

### 14.4 Images

- Decode with `createImageBitmap`, respecting `imageOrientation: 'from-image'`.
- Resize/crop/rotate on `OffscreenCanvas` in a worker.
- Encode with `canvas.convertToBlob({ type, quality })`.
- PNG compression uses a WASM optimiser where available; fall back to canvas re-encode.
- Background removal uses a bundled model or a simple chroma-key path; never a cloud API.
- OCR uses Tesseract.js with language data served from the same origin.
- Limits: reject files over 50 MB by default; warn over 20 MP; cap output dimension at 16,384 px.
- Colour profile handling: convert to sRGB for canvas operations and state the conversion in the UI.

### 14.5 Video and audio

- FFmpeg.wasm loaded on demand from same-origin assets.
- Multithreaded build requires `SharedArrayBuffer`, therefore COOP/COEP headers; fall back to single-thread when unavailable.
- Prefer WebCodecs for simple trim/transcode when supported; FFmpeg for container remux and complex filters.
- Limits: default 500 MB input, 10 minutes duration, 1080p output; configurable by operator.
- Progress parsed from FFmpeg logs; cancellation terminates the worker.
- Audio waveform generated in a worker from decoded PCM; peaks cached per session only.
- Output formats: MP4 (H.264/AAC), WebM (VP9/Opus), MP3, WAV, OGG; availability depends on build.

### 14.6 PDF

| Operation | Implementation |
|---|---|
| Merge/split/rotate/delete/reorder | pdf-lib |
| Images to PDF | pdf-lib + canvas |
| Render to PNG/JPG | pdfjs-dist in a worker |
| Text/image extraction | pdfjs-dist |
| Password protection | Dedicated audited PDF encryption library; never hand-rolled |
| Compression | qpdf WASM (Apache-2.0) plus pdf-lib object-stream optimisation and image downsampling |
| Annotation/form fill | pdf-lib + custom overlay UI |
| Signature verification | pdfjs-dist + bundled trust store; explain limitations |
| PDF to EPUB | pdfjs-dist text extraction + EPUB writer |

Rules:

- Page ranges are parsed and validated before loading.
- Encrypted PDFs prompt for a password and keep it in memory only.
- Output size is measured and displayed.
- Large PDFs (> 100 MB or > 1,000 pages) warn before processing.
- Fonts are embedded or substituted with a documented fallback.

### 14.7 Numbers, dates and network

- Math evaluator uses a custom tokeniser/parser (no `eval`), with functions, constants and explicit errors.
- Decimal arithmetic uses a small precision library for currency-like cases; otherwise IEEE-754 with formatted output.
- Date handling uses `Temporal` where available with a `date-fns` fallback; timezone is explicit in every tool.
- IPv4/IPv6 parsing uses a dedicated library or well-tested custom code; CIDR maths is unit-tested against fixtures.
- MAC vendor data is bundled, versioned and dated; lookup is offline.

### 14.8 QR codes

- Payload builders for text, URL, email, phone, SMS, WiFi and vCard.
- WiFi payloads escape special characters per spec.
- Error-correction level and size are selectable.
- Download as PNG or SVG; no tracking parameters are added.

## 15. Security

### 15.1 Threat model

| Threat | Mitigation |
|---|---|
| User content exfiltration | No network in core tools; CSP `connect-src 'self'`; privacy tests |
| Malicious file parser exploit | Workers, dependency auditing, fuzzing, size limits |
| XSS via rendered HTML/Markdown | Sanitisation, no raw HTML by default, CSP |
| Prototype pollution via parsers | Safe schemas, `Object.create(null)` where relevant, library updates |
| ReDoS | Worker timeouts, input caps, optional RE2 engine |
| Supply-chain compromise | Lockfile, pinned versions, provenance, licence and vulnerability scans |
| Admin takeover (server mode) | Argon2id, secure cookies, CSRF, rate limiting, MFA-ready |
| Data retention leaks | Stateless core; optional modules document retention; no contact message storage |
| Clickjacking | `frame-ancestors 'none'` |
| Cache poisoning | Immutable hashed assets; HTML no-store or short cache; service worker versioning |

### 15.2 Headers

Static host configuration:

```
Content-Security-Policy:
  default-src 'self';
  script-src 'self' 'wasm-unsafe-eval' 'sha256-<build-hash>';
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: blob:;
  media-src 'self' blob:;
  font-src 'self';
  connect-src 'self';
  worker-src 'self' blob:;
  frame-ancestors 'none';
  base-uri 'self';
  form-action 'self';
  object-src 'none'
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Embedder-Policy: require-corp
Cross-Origin-Resource-Policy: same-origin
Referrer-Policy: strict-origin-when-cross-origin
X-Content-Type-Options: nosniff
Permissions-Policy: camera=(self), microphone=(self), geolocation=()
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

Notes:

- `wasm-unsafe-eval` is required for WASM engines; avoid `unsafe-eval`.
- Static mode: Next.js emits inline bootstrap scripts. Generate their SHA-256 hashes at build and list them in `script-src`. If hashing proves impractical for a given build, `'unsafe-inline'` is a documented fallback, but it weakens XSS defence and must be recorded as a risk.
- Server mode: use per-request nonces for inline scripts and remove `unsafe-inline` from `style-src` where possible.
- COOP/COEP enable `SharedArrayBuffer` for multithreaded media. If an operator embeds third-party content, they must relax these headers and accept single-threaded media.

### 15.3 Dependency policy

- Permissive licences: MIT, Apache-2.0, BSD-2/3, ISC and MPL-2.0 (file-level).
- LGPL-2.1/3 components (for example, an FFmpeg build) may be used only as isolated, dynamically loaded WASM modules, with licence notices and relinking provisions. GPL, AGPL and SSPL builds are prohibited.
- Every dependency has a purpose, owner and review date.
- `pnpm audit`, licence scanner and SBOM generation run in CI.
- WASM binaries are pinned by hash and served same-origin.

### 15.4 Optional server security

- Session cookies: `HttpOnly`, `Secure`, `SameSite=Lax`.
- Argon2id password hashing.
- CSRF tokens for state-changing requests.
- Rate limiting on auth, contact and revalidation.
- Role-based access: admin, editor.
- No public registration by default.
- Database least privilege; migrations run as a separate role.
- Backups encrypted; retention documented.

## 16. Privacy

| Data | Where stored | Retention | Shared |
|---|---|---|---|
| Tool input/result | Browser memory | Until reset/close | Never |
| Preferences/favourites | localStorage | Until cleared | Never |
| Tool state | localStorage | Until cleared | Never |
| Analytics events (opt-in) | Analytics endpoint | Operator-defined | No content, no query text |
| Contact message (optional) | Email delivery only | Not stored by default | Operator's mail provider |
| CMS content (optional) | PostgreSQL | Operator-defined | Public if published |

Rules:

- A visible privacy indicator appears on every tool page.
- The privacy page lists every storage key and optional outbound request.
- Self-hosters can disable all outbound requests with `OMNIKIT_TELEMETRY=off`.
- No third-party fonts, scripts, images or CDNs at runtime.
- No fingerprinting, advertising IDs or cross-site tracking.

## 17. PWA and offline

### 17.1 Strategy

- Serwist service worker generated at build.
- Precache: app shell, offline page, search index, P0 tool chunks, icons, core CSS/JS.
- Runtime cache: P1/P2 tool chunks with stale-while-revalidate; WASM/media assets cache-first with explicit versioning.
- Never cache: user files, blobs, results, admin responses, API mutations.
- Cache version derived from build ID; old caches deleted on activate.
- Update flow: detect new version, show non-blocking prompt, activate on user confirmation or next navigation.

### 17.2 Offline behaviour

| Surface | Offline behaviour |
|---|---|
| Home | Works from cache |
| P0 tools | Work from cache after first visit |
| P1/P2 tools | Work if previously visited and cached |
| Search | Works from cached index |
| Favourites/recents | Work locally |
| Blog/CMS/contact | Show offline notice |
| Downloads | Work locally |

### 17.3 Testing

- Playwright offline test: load home and each P0 tool, go offline, reload, run example.
- Verify no stale mixed-version errors after an update.
- Verify cache cleanup and storage quota handling.

## 18. SEO and content

- Static generation for all public routes.
- `generateMetadata` per tool and category; canonical and `hreflang` alternates.
- `sitemap.ts` includes home, categories, tools, static pages and blog.
- `robots.ts` disallows admin and API routes.
- JSON-LD: `WebApplication`, `BreadcrumbList`, optional `FAQPage`.
- Open Graph images generated at build; server mode can generate dynamically.
- RSS for blog.
- `llms.txt` lists the catalogue and canonical descriptions.
- Content is original and reviewed for accuracy; no scraped descriptions.
- Internal linking: category ↔ tool ↔ related tools.
- No doorway pages or keyword stuffing.

## 19. Optional modules

### 19.1 Content

- Default: MDX files in `content/blog`, statically generated.
- Frontmatter: title, description, date, tags, locale, canonical, draft, image.
- Reading time and table of contents generated at build.
- RSS and sitemap entries generated from content.

### 19.2 CMS (server mode)

Data model:

```sql
users (id, email, password_hash, role, created_at)
posts (id, slug, locale, title, description, body, format, status, published_at, updated_at, author_id, seo_title, seo_description, og_image)
tags (id, slug, name)
post_tags (post_id, tag_id)
settings (key, value, updated_at)
sessions (id, user_id, expires_at, created_at)
```

- Drizzle schema and migrations.
- Draft, preview, publish and bulk publish.
- Markdown and rich-text editing with sanitisation.
- Optimistic concurrency using `updated_at`.
- Audit log for publish/unpublish/delete.

### 19.3 Contact

- Validates name, email, subject and message.
- Rate limited by IP and session.
- Sends via SMTP or an email API configured by the operator.
- Does not store messages by default.
- Honeypot plus optional CAPTCHA; no third-party CAPTCHA by default.
- Returns generic success to avoid address enumeration.

### 19.4 Accounts (optional, post-1.0)

- Email/password with Argon2id; optional OIDC.
- Sync favourites, recents and preferences.
- Conflict resolution: last-write-wins per item with timestamps.
- Account deletion removes all user rows within 30 days.
- Core tools remain usable without an account.

### 19.5 Analytics

- Opt-in, privacy-first.
- Self-hosted Umami or Plausible-compatible endpoint.
- Events: `tool_open`, `tool_run`, `tool_success`, `tool_error`, `search_result_click`, `favourite_add`.
- No input, output, file name, query text or identifier.
- Sampling and retention configurable.
- Disabled entirely in static mode unless configured.

## 20. API (server mode)

| Method | Path | Purpose | Auth |
|---|---|---|---|
| `GET` | `/api/health` | Liveness/readiness | None |
| `POST` | `/api/contact` | Contact form | Rate-limited |
| `POST` | `/api/revalidate` | On-demand revalidation | Secret |
| `GET` | `/api/admin/posts` | List posts | Admin/editor |
| `POST` | `/api/admin/posts` | Create post | Admin/editor |
| `PATCH` | `/api/admin/posts/:id` | Update/publish | Admin/editor |
| `DELETE` | `/api/admin/posts/:id` | Delete post | Admin |
| `POST` | `/api/auth/login` | Session login | None, rate-limited |
| `POST` | `/api/auth/logout` | Session logout | Session |

Rules: JSON only, Zod validation, consistent error envelope, no stack traces, CSRF for mutations, audit logging for admin actions.

## 21. Performance engineering

### 21.1 Budgets

| Asset | Budget (compressed) |
|---|---:|
| Home initial JS | 180 KB |
| Shared CSS | 40 KB |
| Simple tool chunk | 60 KB |
| Search index | 150 KB |
| Fonts | 0 KB (system font stack) |
| PDF engine chunk | Loaded on demand |
| Media engine chunk | Loaded on demand |
| OCR language data | Loaded on demand |

### 21.2 Techniques

- Route-level and tool-level code splitting.
- No barrel imports from heavy libraries.
- Dynamic import for every tool and WASM engine.
- Preload search index after first paint; prefetch likely tool chunks on hover/focus.
- Virtualise long lists and large result sets.
- Use `content-visibility` for offscreen sections.
- Compress with Brotli; immutable hashed assets.
- Avoid client-side date/locale libraries when `Intl` suffices.
- Tree-shake icons; import individually.
- Measure with Lighthouse CI and real-device profiles.

### 21.3 Memory

- Enforce file limits before decode.
- Use `createImageBitmap` and release with `close()`.
- Stream large files where APIs allow.
- Terminate workers after jobs.
- Warn at 70% of the estimated memory budget.
- Provide a "reduce size" recovery path on `OUT_OF_MEMORY`.

## 22. Testing strategy

| Layer | Tool | Scope |
|---|---|---|
| Unit | Vitest | Pure logic, parsers, formatters, edge cases |
| Property-based | fast-check | Parsers, converters, encoders |
| Component | Testing Library | Tool shell, validation, keyboard |
| E2E | Playwright | P0 flows, offline, downloads, file handling |
| Accessibility | axe-core | All P0/P1 pages |
| Visual | Playwright screenshots | Design-system regressions |
| Performance | Lighthouse CI | Budgets and regressions |
| Security | Dependency audit, SBOM, fuzzing | Release gate |
| Contract | Registry validation | Catalogue completeness |
| i18n | Custom script | Missing/unused keys, coverage |
| Link | Custom crawler | Internal links and anchors |

### 22.1 Fixtures and golden files

- Each tool has `fixtures/` with input, options and expected output.
- Golden files are reviewed like code.
- Random tools inject a seeded RNG in tests.
- File tools include small binary fixtures (PNG, PDF, WAV, MP4) under 1 MB.
- Conformance suites cover CSV dialects, JSON edge cases, YAML anchors, TOML dates and PDF page ranges.

### 22.2 CI pipeline

1. Install with frozen lockfile.
2. Lint and type-check.
3. Validate registry and translations.
4. Unit and property tests.
5. Build static and server variants.
6. Bundle budget check.
7. Playwright smoke and offline tests.
8. axe accessibility checks.
9. Licence and vulnerability scan.
10. Generate SBOM and build artefacts.

## 23. Observability

- Structured JSON logs in server mode; no user content.
- Request ID propagation.
- Health endpoint reports build version, mode and optional dependency status.
- Metrics (optional): request count, latency, error rate, tool error codes (aggregated, no content).
- Client error reporting is opt-in and strips input/output.
- No third-party monitoring by default.
- Operator runbook documents common failures and recovery.

## 24. Container and deployment

### 24.1 Static image

- Multi-stage build: pnpm install, build, export, copy to unprivileged nginx.
- Non-root user, read-only filesystem, tmpfs for cache.
- Brotli/gzip enabled; immutable caching for hashed assets; no-cache for HTML.
- Security headers in nginx config.
- Health check via `/healthz` static file.
- Target size: ≤ 80 MB.

### 24.2 Server image

- Multi-stage build with Next.js standalone output.
- Node 22 slim, non-root, `tini` as PID 1.
- Read-only filesystem except `/tmp` and cache.
- Health check via `/api/health`.
- Target size: ≤ 200 MB.
- Graceful shutdown on SIGTERM.

### 24.3 Compose

```yaml
services:
  omnikit:
    image: ghcr.io/example/omnikit:latest
    ports:
      - "8080:8080"
    environment:
      OMNIKIT_MODE: static
      OMNIKIT_BASE_PATH: /
      OMNIKIT_DEFAULT_LOCALE: en
      OMNIKIT_TELEMETRY: "off"
    restart: unless-stopped
    read_only: true
    tmpfs:
      - /tmp
```

Server mode adds PostgreSQL and secrets. Optional modules are enabled explicitly.

### 24.4 Configuration

| Variable | Default | Purpose |
|---|---|---|
| `OMNIKIT_MODE` | `static` | `static` or `server` |
| `OMNIKIT_BASE_PATH` | `/` | Reverse-proxy subpath |
| `OMNIKIT_SITE_NAME` | `OmniKit` | Branding |
| `OMNIKIT_DEFAULT_LOCALE` | `en` | Default locale |
| `OMNIKIT_LOCALES` | `en,de,es,fr,pt,zh,ja,uk` | Enabled locales |
| `OMNIKIT_TELEMETRY` | `off` | Opt-in analytics |
| `OMNIKIT_ANALYTICS_URL` | empty | Self-hosted endpoint |
| `OMNIKIT_CONTACT_ENABLED` | `false` | Contact module |
| `OMNIKIT_SMTP_URL` | empty | Mail transport |
| `OMNIKIT_DATABASE_URL` | empty | CMS/accounts |
| `OMNIKIT_AUTH_SECRET` | empty | Session signing |
| `OMNIKIT_REVALIDATE_SECRET` | empty | Revalidation |
| `OMNIKIT_MAX_UPLOAD_MB` | `0` | Server uploads disabled by default |

All variables are validated at startup; server mode fails closed if a required secret is missing.

## 25. Tool authoring workflow

1. Copy `tools/_template`.
2. Add the canonical ID and metadata to `meta.ts`.
3. Implement pure logic in `logic.ts`.
4. Add fixtures and unit tests.
5. Build the UI with shared components in `tool.tsx`.
6. Add a worker if the workload exceeds the main-thread threshold.
7. Add translations for the source locale.
8. Add an example and limitations.
9. Run `pnpm validate`, `pnpm test`, `pnpm build`.
10. Complete the definition of done checklist and request review.

A simple tool should require no changes outside its folder and the generated registry import.

## 26. Architecture decision records

| ADR | Decision | Status |
|---|---|---|
| ADR-001 | Next.js static-first with optional server mode | Accepted |
| ADR-002 | Registry-driven tool catalogue with canonical IDs | Accepted |
| ADR-003 | Local-first processing; no uploads for core tools | Accepted |
| ADR-004 | Workers and lazy WASM for heavy workloads | Accepted |
| ADR-005 | Permissive licence and clean-room implementation | Accepted |
| ADR-006 | Optional server modules isolated from core | Accepted |
| ADR-007 | Typed i18n with English fallback | Accepted |
| ADR-008 | Radix-based design system with CSS tokens | Accepted |
| ADR-009 | MiniSearch prebuilt index | Accepted |
| ADR-010 | Serwist service worker with versioned caches | Accepted |
| ADR-011 | Versioned local storage with Zod validation | Accepted |
| ADR-012 | No runtime third-party CDNs | Accepted |

## 27. Delivery plan

| Phase | Engineering focus | Exit criteria |
|---|---|---|
| M0 Foundation | Monorepo, design system, registry, routing, search, CI, container | One tool end to end; budgets enforced |
| M1 Core | 33 P0 tools, storage, themes, PWA, i18n scaffold, SEO | P0 definition of done; offline and accessibility gates |
| M2 Parity | 56 P1 tools, PDF/image depth, examples, locales | 89 outcomes; two-reference coverage |
| M3 Long tail | 110 P2 tools, media, OCR, niche utilities | 199 outcomes; parity verified |
| M4 Modules | Blog, CMS, contact, accounts, analytics | Modules independently enableable |

## 28. Open technical questions

| ID | Question | Impact |
|---|---|---|
| TQ-01 | Static export vs hybrid rendering for locale routing | Build complexity, SEO |
| TQ-02 | FFmpeg.wasm build variant and codec licensing | Media scope, image size |
| TQ-03 | PDF compression engine selection and licence | PDF compression |
| TQ-04 | OCR language data hosting and size | Offline OCR |
| TQ-05 | COOP/COEP globally vs per-route | SharedArrayBuffer, embedding |
| TQ-06 | Password-protected PDF support limits | PDF feature claims |
| TQ-07 | CMS auth provider and session storage | Server security |
| TQ-08 | Analytics event schema and consent UX | Privacy compliance |
| TQ-09 | Search index size at 199 tools | Performance |
| TQ-10 | Browser support floor for WebCodecs and OffscreenCanvas | Media fallbacks |

## Appendix A — Reference implementation snippets

### A.1 Tool metadata

```ts
// tools/text/case-converter/meta.ts
import { defineTool } from '@omnikit/tool-sdk';

export default defineTool({
  id: 'TXT-01',
  slug: 'case-converter',
  category: 'text',
  phase: 'P0',
  audience: ['general', 'developer'],
  titleKey: 'tools.case-converter.title',
  summaryKey: 'tools.case-converter.summary',
  keywords: ['case', 'camel', 'snake', 'kebab', 'pascal', 'title', 'uppercase', 'lowercase'],
  aliases: ['change case', 'text case'],
  icon: 'case-sensitive',
  capabilities: { files: false, network: false, randomness: false, wasm: false, worker: false },
  examples: [
    {
      nameKey: 'tools.case-converter.examples.basic',
      input: { text: 'hello world', target: 'camel' },
      expected: 'helloWorld',
    },
  ],
  load: () => import('./tool'),
});
```

### A.2 Pure logic

```ts
// tools/text/case-converter/logic.ts
export type CaseTarget =
  | 'lower' | 'upper' | 'title' | 'sentence'
  | 'camel' | 'pascal' | 'snake' | 'kebab'
  | 'constant' | 'dot' | 'path';

export function convertCase(input: string, target: CaseTarget): string {
  const words = input
    .replace(/([a-z0-9])([A-Z])/g, '$1 $2')
    .split(/[^A-Za-z0-9]+/)
    .filter(Boolean)
    .map((word) => word.toLowerCase());

  if (words.length === 0) return '';

  switch (target) {
    case 'lower': return words.join(' ');
    case 'upper': return words.join(' ').toUpperCase();
    case 'title': return words.map(capitalise).join(' ');
    case 'sentence': return capitalise(words.join(' '));
    case 'camel': return words[0] + words.slice(1).map(capitalise).join('');
    case 'pascal': return words.map(capitalise).join('');
    case 'snake': return words.join('_');
    case 'kebab': return words.join('-');
    case 'constant': return words.join('_').toUpperCase();
    case 'dot': return words.join('.');
    case 'path': return words.join('/');
  }
}

function capitalise(word: string): string {
  return word.charAt(0).toUpperCase() + word.slice(1);
}
```

### A.3 Worker wrapper

```ts
// lib/workers/run-job.ts
export async function runJob<TInput, TOutput>(
  worker: Worker,
  payload: TInput,
  options: { timeoutMs: number; onProgress?: (value: number) => void },
): Promise<TOutput> {
  const id = crypto.randomUUID();

  return new Promise<TOutput>((resolve, reject) => {
    const timer = setTimeout(() => {
      worker.terminate();
      reject(new Error('TIMEOUT'));
    }, options.timeoutMs);

    worker.onmessage = (event) => {
      const message = event.data;
      if (message.id !== id) return;
      if (message.type === 'progress') {
        options.onProgress?.(message.value);
      } else if (message.type === 'result') {
        clearTimeout(timer);
        resolve(message.value);
      } else if (message.type === 'error') {
        clearTimeout(timer);
        reject(new Error(message.code));
      }
    };

    worker.postMessage({ id, type: 'run', payload });
  });
}
```

## Appendix B — Definition of done checklist

- [ ] Metadata and canonical ID registered.
- [ ] Stable slug and route.
- [ ] Original example and limitations.
- [ ] Validation for empty, malformed, boundary and oversized input.
- [ ] Copy/download/reset.
- [ ] Progress/cancel for long jobs.
- [ ] No network access with user content.
- [ ] Object URLs revoked.
- [ ] Keyboard, screen reader and mobile verified.
- [ ] Unit, property and E2E tests.
- [ ] Bundle budget respected.
- [ ] Translations complete or fallback-safe.
- [ ] Security review for parsing, crypto or file handling.
- [ ] Documentation updated.
- [ ] No copied code, copy, asset or translation.
