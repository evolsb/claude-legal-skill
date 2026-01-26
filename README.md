# Contract Review Skill for Claude Code

> AI-powered contract analysis with CUAD risk detection, market benchmarks, and lawyer-ready redlines

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![CUAD](https://img.shields.io/badge/CUAD-41%20Categories-green)](https://github.com/TheAtticusProject/cuad)
[![Version](https://img.shields.io/badge/version-3.0.0-blue)]()

## Quick Install

```bash
git clone https://github.com/evolsb/claude-legal-skill ~/.claude/skills/contract-review
```

## Try It

```
Review this NDA - I'm the receiving party
```

---

## What It Does

Analyzes legal contracts and outputs:
- **Risk assessment** with severity ratings (🔴 Critical / 🟡 Important / 🟢 Acceptable)
- **Red flags quick scan** — instant danger sign detection
- **Key terms table** with section references
- **Market standard benchmarks** — how terms compare to industry norms
- **Negotiability ratings** — what's realistic to change given power dynamics
- **Specific redlines** — actual replacement language, not just "negotiate this"
- **Missing provisions** with suggested language to add
- **Internal consistency checks** (broken cross-references, undefined terms)

## Example Output

```markdown
# Contract Review: Acme SaaS Agreement

**Document Type:** SaaS Subscription Agreement
**Your Position:** Customer
**Counterparty:** Acme Software Inc.
**Risk Level:** 🟡 Medium
**Document Status:** Draft

## Red Flags (Quick Scan)

| Flag | Found | Location |
|------|-------|----------|
| Liability cap < 6 months | ⚠️ Yes | Section 10.2 |
| Uncapped indemnification | No | — |
| No termination for convenience | No | — |

## Key Terms

| Term | Value | Location |
|------|-------|----------|
| Liability Cap | 3 months' fees | Section 10.2 |
| Auto-Renewal | 60-day notice | Section 8.2 |

## Risk Analysis

### 🔴 Critical

**Limitation of Liability** (Section 10.2)
> "Liability shall not exceed fees paid in the preceding three (3) months"

- **Issue:** 3-month cap is below market standard
- **Market Standard:** 12 months' fees
- **Negotiability:** Medium — most vendors accept 6-12 months
- **Redline:** Change "three (3) months" → "twelve (12) months"
- **Fallback:** Accept 6 months as compromise

### 🟢 Reviewed & Acceptable

| Category | Notes |
|----------|-------|
| Data Ownership | ✓ Customer owns all data |
| Governing Law | ✓ Delaware — standard |

## Negotiation Priority

| # | Issue | Ask | Negotiability |
|---|-------|-----|---------------|
| 1 | Liability cap | 12 months | Medium |
| 2 | Termination | Mutual rights | High |
```

---

## Features

### Position-Aware Review
Tell it which party you are (customer, vendor, buyer, seller, receiving party) — the skill adjusts what it flags as risky.

### Document Type Checklists
Specialized checklists for each contract type:
- **NDA** — confidentiality term, non-solicitation, standstill, destruction certification
- **SaaS/MSA** — SLA, data export, suspension rights, price caps
- **Payment/Merchant** — reserves, chargebacks, network rules, auto-debit
- **M&A** — earnouts, escrow, rep survival, sandbagging
- **Finder/Broker** — fee tails, covered buyer definitions, joint representation

### Market Standard Benchmarks
Compares terms to industry norms with clear thresholds:

| Provision | Standard | Yellow | Red |
|-----------|----------|--------|-----|
| Liability cap | 12 months | 6-11 mo | <6 mo |
| Auto-renewal notice | 90+ days | 60-89 | <60 |
| Non-compete | 1-2 years | 3-4 years | 5+ |
| Rep survival (M&A) | 12-18 mo | 24-30 mo | 36+ mo |

### Negotiability Ratings
Tells you what's actually changeable:
- **High** — Mutual termination, cure periods, data export
- **Medium** — Liability cap increases, price caps
- **Low** — Network rules, regulatory requirements

### Red Flags Quick Scan
Instant detection of danger signs:
- Liability cap < 6 months
- Uncapped indemnification
- Unilateral amendment rights
- Perpetual obligations
- Offshore jurisdiction (BVI, Cayman)

### Jurisdiction Awareness
Flags when governing law affects enforceability:
- Non-competes void in CA/ND/OK/MN
- Delaware vs NY vs CA implications
- Offshore jurisdiction cost/enforcement concerns

### M&A Support
Special handling for acquisition agreements:
- Earnout mechanics and measurement
- Rep & warranty survival periods
- Working capital adjustments
- Escrow/holdback provisions
- Employment comp in deal value calculations

---

## Installation Options

### Standard (Recommended)
```bash
git clone https://github.com/evolsb/claude-legal-skill ~/.claude/skills/contract-review
```

### Development (Symlink)
```bash
git clone https://github.com/evolsb/claude-legal-skill ~/Developer/claude-legal-skill
ln -s ~/Developer/claude-legal-skill ~/.claude/skills/contract-review
```

---

## Usage Examples

```
Review this NDA for red flags - I'm the receiving party

Analyze the indemnification in this MSA - I'm the vendor

What are the termination provisions? I'm the customer.

Review this acquisition agreement - I'm the seller

Check this merchant agreement - what's my chargeback exposure?
```

---

## Limitations

- **Not legal advice** — always have material terms reviewed by qualified counsel
- **US law focus** — analysis defaults to US; provisions vary by jurisdiction
- **Context window** — very long contracts may need section-by-section review

## Accuracy

Based on ContractEval benchmarks, Claude achieves F1 ~0.62 on clause extraction. Best for first-pass review and issue flagging — not a replacement for attorney review on material deals.

---

## Credits

- [CUAD Dataset](https://github.com/TheAtticusProject/cuad) — Atticus Project (NeurIPS 2021)
- [LegalBench](https://hazyresearch.stanford.edu/legalbench/) — Stanford HAI
- [ContractEval](https://arxiv.org/abs/2303.07389) — Contract understanding benchmarks

## License

MIT — see [LICENSE](LICENSE)
