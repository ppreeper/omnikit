# Tool Equivalence Map

**Project:** OmniKit (working name)  
**Version:** 1.0  
**Status:** Draft for review  
**Date:** 2026-10-18

## 1. Purpose

This document maps every tool function offered by the three anonymised reference suites onto a single canonical OmniKit catalogue. It answers three questions:

1. Which tools are functionally **the same** across references?
2. Which tools are **different** or unique to one reference?
3. What must OmniKit build to reach **complete functional parity**?

The reference suites are labelled throughout as **Reference A**, **Reference B** and **Reference C**. Their names are deliberately omitted. Full per-reference inventories are in [Competitive Analysis](./competitive-analysis.md).

## 2. Method and equivalence rules

- A canonical row represents one **user outcome**, not one codebase or one UI screen.
- Two tools are considered the same when they produce the same primary outcome, even if one is broader or implemented differently.
- **✓** means the outcome is available as a dedicated tool.
- **◐** means the outcome exists only as a narrower mode, an option inside another tool, or a partially integrated feature.
- **—** means the outcome is absent.
- Source identifiers are kept in the build data for traceability and are validated by a script: every tool in every reference appears at least once.
- A single reference tool can satisfy more than one canonical row (for example a combined JSON workbench or a five-operation PDF suite). This is noted in the row notes.

## 3. Source catalogue sizes

| Reference | Architecture | Tools | Categories | Locales |
|---|---|---:|---:|---:|
| A | Vue single-page application, developer-focused, PWA-first | 86 | 10 | 9 |
| B | React single-page application, media-heavy, everyday-task focus | 133 | 14 | 11 |
| C | Next.js server-rendered application, content and PDF focus | 14 top-level (19 including PDF operations) | 6 | 2 |
| **Total source instances** | | **233** | | |

After de-duplication, these 233 source instances resolve to **199 canonical tool outcomes**.

## 4. Summary of overlap

| Presence | Canonical outcomes | Share |
|---|---:|---:|
| All three references | 9 | 4.5% |
| A + B only | 10 | 5.0% |
| A + C only | 7 | 3.5% |
| B + C only | 8 | 4.0% |
| A only | 61 | 30.7% |
| B only | 101 | 50.8% |
| C only | 3 | 1.5% |
| **Total** | **199** | **100%** |

### 4.1 Universal outcomes (present in all three)

These are the non-negotiable core of the product. They must be P0 and must ship in the first release.


- **Text diff / compare** (`TXT-12`)
- **JSON format and validate** (`DAT-01`)
- **JSON minify** (`DAT-02`)
- **JSON diff** (`DAT-04`)
- **JSON to CSV** (`DAT-05`)
- **XML format** (`DAT-20`)
- **Base64 text encode/decode** (`ENC-01`)
- **URL encode/decode** (`ENC-03`)
- **Cron parser and builder** (`DEV-09`)

### 4.2 Shared by two references

**A + B only:** Case conversion, Text statistics, Slugify, Unicode escape / unescape, JSON to XML, URL parser and editor, Bcrypt hash and compare, Random port generator, QR code generator, Unix timestamp converter

**A + C only:** Lorem ipsum generator, Markdown to HTML, JSON to YAML, Hash generator (MD5/SHA), UUID generator and validator, Regex tester, Color converter

**B + C only:** CSV / TSV to JSON, XML validate, Merge PDF, Split PDF, Compress PDF, PDF editor, Images to PDF, PDF to PNG / images

### 4.3 Unique to one reference

**A only:** String obfuscator, Numeronym generator, ASCII text art, Emoji picker, Email normalizer, HTML entities, HTML WYSIWYG editor, Text and binary, NATO phonetic alphabet, XML to JSON, YAML to JSON, JSON to TOML, TOML to JSON, YAML to TOML, TOML to YAML, YAML format / view, Column / list converter, Phone parser and formatter, IBAN validator and parser, HTTP status codes, MIME type lookup, Base64 file encode/decode, HMAC generator, Symmetric encryption / decryption, RSA key pair generator, BIP39 mnemonic, Random token generator, ULID generator, Password strength analyser, OTP generator and validator, JWT parser, Basic auth header generator, WiFi QR code generator, SVG placeholder generator, Camera recorder, Device information, Meta tag / Open Graph generator, Keycode information, User-agent parser, Safelink decoder, Git cheatsheet, Regex cheatsheet, SQL formatter, Chmod calculator, Docker run to Compose, Benchmark builder, Date-time converter, ETA calculator, Percentage calculator, Math expression evaluator, Chronometer / stopwatch, Temperature converter, Integer base converter, Roman numeral converter, IPv4 subnet calculator, IPv4 address converter, IPv4 range expander, IPv6 ULA generator, MAC address generator, MAC address lookup, PDF signature verification

**C only:** JSON tree viewer, JSON Schema generator, WSDL / XML tree and XPath

**B only (largest gap):** Randomize case, Reverse text, Repeat text, Truncate text, Split text, Join text, Extract substring, Find and replace, Remove duplicate lines, Palindrome check, Create palindrome, Morse code, ROT13, Rotate text, Censor text, Quote / unquote text, Hidden character detector, Email extractor, Table text converter, JSON escape / unescape, JSON stringify, JSON sort, CSV to TSV, CSV to XML, CSV to YAML, Change CSV separator, Transpose CSV, Insert CSV columns, Swap CSV columns, Find incomplete CSV records, Password generator, Random number generator, Time between dates, Leap year checker, Days and hours conversion, Seconds and time conversion, Time to decimal, Time to seconds, Truncate clock time, Weekday calculator, Discord timestamp generator, Byte / data unit converter, Number to words, Arithmetic sequence, Number sequence generator, Sum numbers, Ohm's law calculator, Cable voltage drop, Sphere area, Sphere volume, Slackline tension, Group or chunk list, Duplicate list items, Most popular list items, Unique list items, Reverse list, Rotate list, Shuffle list, Sort list, Truncate list, Unwrap list, Wrap list, Resize image, Compress image, Compress PNG, Crop image, Rotate image, Image editor, Change image colours, Change image opacity, Create transparent PNG, Remove image background, Watermark images, Split image into pages, JPG to PNG, Image to JPG, Image to WebP, Image to text (OCR), Trim video, Merge videos, Resize video, Crop video, Rotate video, Flip video, Compress video, Change video speed, Loop video, Add audio to video, Extract audio from video, Video to GIF, Images to GIF, Change GIF speed, Trim audio, Merge audio, Loop audio, Change audio speed, Audio format converter, Rotate PDF pages, Protect PDF with password, Extract images from PDF, PDF to EPUB

## 5. Full canonical matrix

Legend: ✓ dedicated · ◐ partial/narrower/integrated · — absent. Phase: **P0** first release, **P1** parity release, **P2** long-tail/media release.


### 5.1 Text & strings (35)

Everyday string manipulation, comparison, generation and formatting.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| TXT-01 | Case conversion | ✓ | ◐ | — | P0 | Full camel/snake/kebab/title conversion; reference B covers uppercase only. |
| TXT-02 | Randomize case | — | ✓ | — | P2 |  |
| TXT-03 | Reverse text | — | ✓ | — | P2 |  |
| TXT-04 | Repeat text | — | ✓ | — | P2 |  |
| TXT-05 | Truncate text | — | ✓ | — | P2 |  |
| TXT-06 | Split text | — | ✓ | — | P1 |  |
| TXT-07 | Join text | — | ✓ | — | P1 |  |
| TXT-08 | Extract substring | — | ✓ | — | P2 |  |
| TXT-09 | Find and replace | — | ✓ | — | P0 | Plain text and regular-expression replacement. |
| TXT-10 | Remove duplicate lines | — | ✓ | — | P0 |  |
| TXT-11 | Text statistics | ✓ | ✓ | — | P0 | Characters, words, lines, sentences, frequency. |
| TXT-12 | Text diff / compare | ✓ | ✓ | ✓ | P0 | Unified, side-by-side, ignore whitespace/case. |
| TXT-13 | Lorem ipsum generator | ✓ | — | ✓ | P1 |  |
| TXT-14 | Slugify | ✓ | ✓ | — | P0 |  |
| TXT-15 | String obfuscator | ✓ | — | — | P2 | Reveal-on-hover style masking for secrets. |
| TXT-16 | Numeronym generator | ✓ | — | — | P2 |  |
| TXT-17 | ASCII text art | ✓ | — | — | P2 |  |
| TXT-18 | Emoji picker | ✓ | — | — | P2 | Unicode names and code points. |
| TXT-19 | Palindrome check | — | ✓ | — | P2 |  |
| TXT-20 | Create palindrome | — | ✓ | — | P2 |  |
| TXT-21 | Morse code | — | ✓ | — | P2 |  |
| TXT-22 | ROT13 | — | ✓ | — | P2 |  |
| TXT-23 | Rotate text | — | ✓ | — | P2 |  |
| TXT-24 | Censor text | — | ✓ | — | P2 |  |
| TXT-25 | Quote / unquote text | — | ✓ | — | P2 |  |
| TXT-26 | Hidden character detector | — | ✓ | — | P1 | Bidi override and zero-width character inspection. |
| TXT-27 | Email extractor | — | ✓ | — | P1 |  |
| TXT-28 | Email normalizer | ✓ | — | — | P1 |  |
| TXT-29 | Table text converter | — | ✓ | — | P1 | Markdown, CSV, TSV, HTML, SQL rows, JSON. |
| TXT-30 | HTML entities | ✓ | — | — | P1 | Escape and unescape. |
| TXT-31 | HTML WYSIWYG editor | ✓ | — | — | P2 |  |
| TXT-32 | Markdown to HTML | ✓ | — | ✓ | P0 | Live preview, sanitized output, print/PDF. |
| TXT-33 | Text and binary | ✓ | — | — | P2 |  |
| TXT-34 | NATO phonetic alphabet | ✓ | — | — | P2 |  |
| TXT-35 | Unicode escape / unescape | ✓ | ✓ | — | P1 |  |

### 5.2 Data formats (35)

JSON, YAML, TOML, XML and CSV/TSV structured-data work.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| DAT-01 | JSON format and validate | ✓ | ✓ | ✓ | P0 | Reference C integrates format, validate, tree and converters. |
| DAT-02 | JSON minify | ✓ | ✓ | ◐ | P0 |  |
| DAT-03 | JSON tree viewer | — | — | ✓ | P1 | Expandable tree with path copy. |
| DAT-04 | JSON diff | ✓ | ✓ | ◐ | P0 |  |
| DAT-05 | JSON to CSV | ✓ | ✓ | ◐ | P0 |  |
| DAT-06 | CSV / TSV to JSON | — | ✓ | ◐ | P0 |  |
| DAT-07 | JSON to XML | ✓ | ✓ | — | P1 |  |
| DAT-08 | XML to JSON | ✓ | — | — | P1 |  |
| DAT-09 | JSON to YAML | ✓ | — | ◐ | P1 |  |
| DAT-10 | YAML to JSON | ✓ | — | — | P1 |  |
| DAT-11 | JSON to TOML | ✓ | — | — | P2 |  |
| DAT-12 | TOML to JSON | ✓ | — | — | P2 |  |
| DAT-13 | YAML to TOML | ✓ | — | — | P2 |  |
| DAT-14 | TOML to YAML | ✓ | — | — | P2 |  |
| DAT-15 | YAML format / view | ✓ | — | — | P1 |  |
| DAT-16 | JSON escape / unescape | — | ✓ | — | P2 |  |
| DAT-17 | JSON stringify | — | ✓ | — | P2 |  |
| DAT-18 | JSON sort | — | ✓ | — | P1 | Sort arrays of objects by key. |
| DAT-19 | JSON Schema generator | — | — | ✓ | P1 | Draft 2020-12 and Draft-07 output. |
| DAT-20 | XML format | ✓ | ✓ | ✓ | P0 |  |
| DAT-21 | XML validate | — | ✓ | ✓ | P1 |  |
| DAT-22 | WSDL / XML tree and XPath | — | — | ✓ | P2 |  |
| DAT-23 | CSV to TSV | — | ✓ | — | P1 |  |
| DAT-24 | CSV to XML | — | ✓ | — | P2 |  |
| DAT-25 | CSV to YAML | — | ✓ | — | P2 |  |
| DAT-26 | Change CSV separator | — | ✓ | — | P1 |  |
| DAT-27 | Transpose CSV | — | ✓ | — | P1 |  |
| DAT-28 | Insert CSV columns | — | ✓ | — | P2 |  |
| DAT-29 | Swap CSV columns | — | ✓ | — | P2 |  |
| DAT-30 | Find incomplete CSV records | — | ✓ | — | P2 |  |
| DAT-31 | Column / list converter | ✓ | — | — | P2 | Prefix, suffix, transpose and column transforms. |
| DAT-32 | Phone parser and formatter | ✓ | — | — | P1 |  |
| DAT-33 | IBAN validator and parser | ✓ | — | — | P2 |  |
| DAT-34 | HTTP status codes | ✓ | — | — | P2 |  |
| DAT-35 | MIME type lookup | ✓ | — | — | P2 |  |

### 5.3 Encoding & security (24)

Encoding, hashing, encryption, identifiers, passwords and QR codes.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| ENC-01 | Base64 text encode/decode | ✓ | ✓ | ✓ | P0 |  |
| ENC-02 | Base64 file encode/decode | ✓ | — | — | P1 |  |
| ENC-03 | URL encode/decode | ✓ | ✓ | ✓ | P0 |  |
| ENC-04 | URL parser and editor | ✓ | ✓ | — | P1 |  |
| ENC-05 | Hash generator (MD5/SHA) | ✓ | — | ✓ | P0 | MD5, SHA-1/2/3 family. |
| ENC-06 | Bcrypt hash and compare | ✓ | ✓ | — | P1 |  |
| ENC-07 | HMAC generator | ✓ | — | — | P1 |  |
| ENC-08 | Symmetric encryption / decryption | ✓ | — | — | P1 | AES, TripleDES, Rabbit; passphrase based. |
| ENC-09 | RSA key pair generator | ✓ | — | — | P2 |  |
| ENC-10 | BIP39 mnemonic | ✓ | — | — | P2 |  |
| ENC-11 | Random token generator | ✓ | — | — | P1 |  |
| ENC-12 | UUID generator and validator | ✓ | — | ✓ | P0 | v1/v4/v7 where supported; bulk and validation. |
| ENC-13 | ULID generator | ✓ | — | — | P2 |  |
| ENC-14 | Password strength analyser | ✓ | — | — | P1 |  |
| ENC-15 | Password generator | — | ✓ | — | P0 |  |
| ENC-16 | OTP generator and validator | ✓ | — | — | P2 | TOTP/HOTP. |
| ENC-17 | JWT parser | ✓ | — | — | P0 | Decode header, payload, signature; no verification by default. |
| ENC-18 | Basic auth header generator | ✓ | — | — | P2 |  |
| ENC-19 | Random port generator | ✓ | ✓ | — | P1 |  |
| ENC-20 | Random number generator | — | ✓ | — | P1 |  |
| ENC-21 | QR code generator | ✓ | ✓ | — | P0 | URL, text, email, phone, SMS, WiFi, vCard. |
| ENC-22 | WiFi QR code generator | ✓ | — | — | P2 |  |
| ENC-23 | SVG placeholder generator | ✓ | — | — | P2 |  |
| ENC-24 | Camera recorder | ✓ | — | — | P2 | Photo and video capture via MediaDevices. |

### 5.4 Web & developer utilities (13)

Browser, web, DevOps and developer reference helpers.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| DEV-01 | Device information | ✓ | — | — | P2 |  |
| DEV-02 | Meta tag / Open Graph generator | ✓ | — | — | P2 |  |
| DEV-03 | Keycode information | ✓ | — | — | P2 |  |
| DEV-04 | User-agent parser | ✓ | — | — | P2 |  |
| DEV-05 | Safelink decoder | ✓ | — | — | P2 |  |
| DEV-06 | Git cheatsheet | ✓ | — | — | P2 |  |
| DEV-07 | Regex tester | ✓ | — | ✓ | P0 | Live match highlighting, groups, replacement preview, common patterns. |
| DEV-08 | Regex cheatsheet | ✓ | — | — | P2 |  |
| DEV-09 | Cron parser and builder | ✓ | ✓ | ✓ | P0 | Human-readable explanation and next-run preview. |
| DEV-10 | SQL formatter | ✓ | — | — | P1 | Dialect-aware formatting. |
| DEV-11 | Chmod calculator | ✓ | — | — | P2 |  |
| DEV-12 | Docker run to Compose | ✓ | — | — | P2 |  |
| DEV-13 | Benchmark builder | ✓ | — | — | P2 |  |

### 5.5 Date & time (12)

Timestamp, duration, calendar and schedule calculations.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| TIM-01 | Date-time converter | ✓ | — | — | P1 | ISO, RFC, Unix, local and custom formats. |
| TIM-02 | Unix timestamp converter | ◐ | ✓ | — | P0 |  |
| TIM-03 | Time between dates | — | ✓ | — | P1 |  |
| TIM-04 | Leap year checker | — | ✓ | — | P2 |  |
| TIM-05 | Days and hours conversion | — | ✓ | — | P2 |  |
| TIM-06 | Seconds and time conversion | — | ✓ | — | P2 |  |
| TIM-07 | Time to decimal | — | ✓ | — | P2 |  |
| TIM-08 | Time to seconds | — | ✓ | — | P2 |  |
| TIM-09 | Truncate clock time | — | ✓ | — | P2 |  |
| TIM-10 | Weekday calculator | — | ✓ | — | P2 |  |
| TIM-11 | Discord timestamp generator | — | ✓ | — | P2 |  |
| TIM-12 | ETA calculator | ✓ | — | — | P2 |  |

### 5.6 Numbers & math (16)

Arithmetic, unit conversion, engineering and measurement calculators.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| NUM-01 | Percentage calculator | ✓ | — | — | P0 |  |
| NUM-02 | Math expression evaluator | ✓ | — | — | P0 |  |
| NUM-03 | Chronometer / stopwatch | ✓ | — | — | P2 |  |
| NUM-04 | Temperature converter | ✓ | — | — | P2 |  |
| NUM-05 | Byte / data unit converter | — | ✓ | — | P1 | SI and binary units. |
| NUM-06 | Number to words | — | ✓ | — | P2 |  |
| NUM-07 | Arithmetic sequence | — | ✓ | — | P2 |  |
| NUM-08 | Number sequence generator | — | ✓ | — | P2 |  |
| NUM-09 | Sum numbers | — | ✓ | — | P1 |  |
| NUM-10 | Ohm's law calculator | — | ✓ | — | P2 |  |
| NUM-11 | Cable voltage drop | — | ✓ | — | P2 |  |
| NUM-12 | Sphere area | — | ✓ | — | P2 |  |
| NUM-13 | Sphere volume | — | ✓ | — | P2 |  |
| NUM-14 | Slackline tension | — | ✓ | — | P2 |  |
| NUM-15 | Integer base converter | ✓ | — | — | P1 |  |
| NUM-16 | Roman numeral converter | ✓ | — | — | P2 |  |

### 5.7 Network (6)

IPv4/IPv6 and MAC address utilities.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| NET-01 | IPv4 subnet calculator | ✓ | — | — | P2 |  |
| NET-02 | IPv4 address converter | ✓ | — | — | P2 |  |
| NET-03 | IPv4 range expander | ✓ | — | — | P2 |  |
| NET-04 | IPv6 ULA generator | ✓ | — | — | P2 |  |
| NET-05 | MAC address generator | ✓ | — | — | P2 |  |
| NET-06 | MAC address lookup | ✓ | — | — | P2 | Vendor lookup uses a bundled or user-supplied database. |

### 5.8 Design (1)

Colour conversion and design helpers.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| DSN-01 | Color converter | ✓ | — | ✓ | P0 | HEX, RGB, HSL, CMYK; visual picker and contrast hints. |

### 5.9 Lists (11)

Line/column list transformations.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| LST-01 | Group or chunk list | — | ✓ | — | P2 | Reference B exposes grouping; an unexposed chunk implementation also exists. OmniKit exposes both. |
| LST-02 | Duplicate list items | — | ✓ | — | P2 |  |
| LST-03 | Most popular list items | — | ✓ | — | P2 |  |
| LST-04 | Unique list items | — | ✓ | — | P1 |  |
| LST-05 | Reverse list | — | ✓ | — | P2 |  |
| LST-06 | Rotate list | — | ✓ | — | P2 |  |
| LST-07 | Shuffle list | — | ✓ | — | P1 |  |
| LST-08 | Sort list | — | ✓ | — | P1 |  |
| LST-09 | Truncate list | — | ✓ | — | P2 |  |
| LST-10 | Unwrap list | — | ✓ | — | P2 |  |
| LST-11 | Wrap list | — | ✓ | — | P2 |  |

### 5.10 Images (16)

Raster image editing and conversion.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| IMG-01 | Resize image | — | ✓ | — | P0 | Pixels or percentage; preserve aspect ratio. |
| IMG-02 | Compress image | — | ✓ | — | P0 | Quality and format controls. |
| IMG-03 | Compress PNG | — | ✓ | — | P1 | Lossless and lossy PNG optimisation. |
| IMG-04 | Crop image | — | ✓ | — | P0 |  |
| IMG-05 | Rotate image | — | ✓ | — | P1 |  |
| IMG-06 | Image editor | — | ✓ | — | P1 | Crop, rotate, annotate, adjust, export. |
| IMG-07 | Change image colours | — | ✓ | — | P2 |  |
| IMG-08 | Change image opacity | — | ✓ | — | P2 |  |
| IMG-09 | Create transparent PNG | — | ✓ | — | P2 |  |
| IMG-10 | Remove image background | — | ✓ | — | P2 |  |
| IMG-11 | Watermark images | — | ✓ | — | P2 |  |
| IMG-12 | Split image into pages | — | ✓ | — | P2 |  |
| IMG-13 | JPG to PNG | — | ✓ | — | P0 |  |
| IMG-14 | Image to JPG | — | ✓ | — | P1 |  |
| IMG-15 | Image to WebP | — | ✓ | — | P1 |  |
| IMG-16 | Image to text (OCR) | — | ✓ | — | P2 |  |

### 5.11 Video (14)

Video and animated GIF editing.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| VID-01 | Trim video | — | ✓ | — | P1 |  |
| VID-02 | Merge videos | — | ✓ | — | P1 |  |
| VID-03 | Resize video | — | ✓ | — | P1 |  |
| VID-04 | Crop video | — | ✓ | — | P2 |  |
| VID-05 | Rotate video | — | ✓ | — | P1 |  |
| VID-06 | Flip video | — | ✓ | — | P2 |  |
| VID-07 | Compress video | — | ✓ | — | P1 |  |
| VID-08 | Change video speed | — | ✓ | — | P2 |  |
| VID-09 | Loop video | — | ✓ | — | P2 |  |
| VID-10 | Add audio to video | — | ✓ | — | P2 |  |
| VID-11 | Extract audio from video | — | ✓ | — | P2 |  |
| VID-12 | Video to GIF | — | ✓ | — | P1 |  |
| VID-13 | Images to GIF | — | ✓ | — | P2 |  |
| VID-14 | Change GIF speed | — | ✓ | — | P2 |  |

### 5.12 Audio (5)

Audio editing and conversion.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| AUD-01 | Trim audio | — | ✓ | — | P2 |  |
| AUD-02 | Merge audio | — | ✓ | — | P2 |  |
| AUD-03 | Loop audio | — | ✓ | — | P2 |  |
| AUD-04 | Change audio speed | — | ✓ | — | P2 |  |
| AUD-05 | Audio format converter | — | ✓ | — | P2 |  |

### 5.13 PDF & documents (11)

PDF manipulation, conversion and inspection.

| ID | Capability | A | B | C | Phase | Notes |
|---|---|:--:|:--:|:--:|:--:|---|
| PDF-01 | Merge PDF | — | ✓ | ✓ | P0 |  |
| PDF-02 | Split PDF | — | ✓ | ✓ | P0 |  |
| PDF-03 | Compress PDF | — | ✓ | ✓ | P0 | Licence-safe object-stream optimisation and image downsampling. |
| PDF-04 | PDF editor | — | ✓ | ◐ | P1 | Annotate, highlight, form fill, page rotate/delete/reorder. |
| PDF-05 | Images to PDF | — | ✓ | ✓ | P1 |  |
| PDF-06 | PDF to PNG / images | — | ✓ | ✓ | P1 |  |
| PDF-07 | Rotate PDF pages | — | ✓ | — | P1 |  |
| PDF-08 | Protect PDF with password | — | ✓ | — | P1 |  |
| PDF-09 | Extract images from PDF | — | ✓ | — | P2 |  |
| PDF-10 | PDF to EPUB | — | ✓ | — | P2 |  |
| PDF-11 | PDF signature verification | ✓ | — | — | P2 |  |

## 6. What "complete parity" means

OmniKit reaches functional parity when:

1. All **199** canonical outcomes are implemented and discoverable.
2. All **233** source tool instances have a documented equivalent (the validation script proves this).
3. Every **◐ partial** outcome listed above is upgraded to full support — specifically:
   - full case conversion beyond uppercase (TXT-01);
   - JSON tree, JSON Schema, JSON↔CSV/YAML conversions that are currently buried inside a single formatter (DAT-03, DAT-06, DAT-09, DAT-19);
   - XML validation and WSDL/XPath inspection as first-class tools (DAT-21, DAT-22);
   - PDF page editing beyond rotate/delete, including annotation and form filling (PDF-04);
   - Unix timestamp handling exposed as a dedicated converter as well as inside the general date-time tool (TIM-02).
4. Platform features from all three references are present: search, categories, favourites, recent tools, examples, per-tool state, PWA/offline, light/dark themes, i18n, SEO-ready pages, and optional content/CMS modules.
5. Every tool passes the definition of done in the PRD and technical specification.

## 7. Build implications

| Category | Canonical rows | Dominant source | Main implementation risk |
|---|---:|---|---|
| Text & strings | 35 | B | Low; pure functions and Unicode edge cases |
| Data formats | 35 | A + B | Medium; format fidelity and large documents |
| Encoding & security | 24 | A | Medium; cryptographic correctness and WebCrypto limits |
| Web & developer utilities | 13 | A | Low–medium; reference data freshness |
| Date & time | 12 | A + B | Low; timezone and locale correctness |
| Numbers & math | 16 | A + B | Low; formula correctness |
| Network | 6 | A | Low; bundled lookup data |
| Design | 1 | A + C | Low |
| Lists | 11 | B | Low |
| Images | 16 | B | High; memory, codecs and colour accuracy |
| Video | 14 | B | High; FFmpeg/WASM size, CPU and memory |
| Audio | 5 | B | High; codec support and long files |
| PDF | 11 | B + C | High; encryption, compression engine and fidelity |

**Recommended sequencing:** build the platform and the 199-row registry first, then deliver P0 (text, data and encoding core plus essential image and PDF operations), P1 (developer, calculation and remaining two-reference coverage), and finally P2 (advanced media, OCR, niche generators and reference data). Media and PDF engines should be lazy-loaded so they never affect the home-page or text-tool budget.

## 8. Traceability

The machine-readable mapping is maintained alongside this document in [`docs/tool-map.data.json`](./tool-map.data.json). Every row carries a stable ID; requirements, tests, analytics events and documentation pages should reference these IDs.
