# OmniKit Documentation

**Project:** OmniKit (working name)  
**Status:** Planning  
**Last updated:** 2026-10-18

OmniKit is a self-hosted web application offering a broad catalogue of everyday online tools. It is designed to be complete, private, fast and easy to operate. Core tools run entirely in the browser; optional server modules add content, contact and administration features.

## Document map

| Document | What it answers | Audience |
|---|---|---|
| [Competitive Analysis](./competitive-analysis.md) | What the three reference suites offer, how they differ, and where the opportunity is | Product, design, engineering |
| [Tool Equivalence Map](./tool-map.md) | Every tool outcome, which references provide it, and when OmniKit will build it | Product, engineering, QA |
| [Product Requirements Document](./PRD.md) | What the product must do, for whom, with what acceptance criteria | Product, design, engineering, stakeholders |
| [Technical Specification](./technical-spec.md) | How the product is built, secured, tested and deployed | Engineering, operations, security |

## Reading order

1. **Competitive Analysis** — understand the market and the evidence.
2. **Tool Equivalence Map** — understand the 199-outcome catalogue and the parity target.
3. **PRD** — understand scope, priorities, requirements and release gates.
4. **Technical Specification** — understand architecture, contracts, pipelines and operations.

## Key numbers

| Metric | Value |
|---|---:|
| Reference tool instances | 233 |
| Canonical tool outcomes | 199 |
| Categories | 13 |
| Outcomes shared by all references | 9 |
| Outcomes unique to one reference | 182 |
| P0 outcomes (release 1.0) | 33 |
| P1 outcomes (release 1.1) | 56 |
| P2 outcomes (release 1.2+) | 110 |

## Conventions

- The reference products are always called **Reference A**, **Reference B** and **Reference C**. Their names, URLs and brands do not appear in project documentation.
- Tool outcomes use stable IDs (`TXT-01`, `DAT-01`, `ENC-01`, and so on) defined in the [Tool Equivalence Map](./tool-map.md).
- Priority means:
  - **P0** — release 1.0; universal or high-frequency.
  - **P1** — release 1.1; completes two-reference coverage and major category depth.
  - **P2** — release 1.2+; long-tail, advanced media and niche utilities.
- Requirements use `FR-*` for functional, `NFR-*` for non-functional, `G-*` for goals, `OQ-*` for open questions and `TQ-*` for technical questions.
- All documentation is original work. No text, assets, translations or code are copied from the reference products.
- The working name **OmniKit** is provisional pending trademark clearance (OQ-01).

## Current status

| Area | Status |
|---|---|
| Competitive research | Complete |
| Canonical tool mapping | Complete, validated at 86/86, 133/133 and 14/14 |
| Product requirements | Draft for review |
| Technical specification | Draft for review |
| Implementation | Not started |
| Machine-readable catalogue | Complete (`docs/tool-map.data.json`) |

## Next actions

1. Review and sign off the PRD scope and P0 composition.
2. Confirm the product name and licence (OQ-01, OQ-02).
3. Resolve the technical questions that affect architecture (TQ-01 to TQ-05).
4. Keep `docs/tool-map.data.json` in sync whenever the canonical matrix changes.
5. Start milestone M0: repository foundation, design system, registry and CI.

## Contributing to the documentation

- Keep the tool map authoritative; update it before changing scope.
- Reference canonical IDs in requirements, tests and analytics events.
- Do not introduce reference product names, links or copied descriptions.
- Prefer measurable acceptance criteria over adjectives.
- Record decisions as ADRs in the technical specification.
