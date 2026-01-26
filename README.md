# Contract Review Skill for Claude Code

> AI-powered contract analysis with CUAD risk detection and lawyer-ready redlines

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![CUAD](https://img.shields.io/badge/CUAD-41%20Categories-green)](https://github.com/TheAtticusProject/cuad)

## Quick Install

```bash
git clone https://github.com/evolsb/claude-legal-skill ~/.claude/skills/contract-review
```

## Try It

```
Review this NDA - I'm the customer
```

---

## What It Does

Analyzes legal contracts and outputs:
- **Risk assessment** with severity ratings (🔴 Critical / 🟡 Important / 🟢 Acceptable)
- **Key terms table** with section references
- **Specific redlines** — actual replacement language, not just "negotiate this"
- **Missing provisions** with suggested language to add
- **Internal consistency checks** (broken cross-references, undefined terms)

## Example Output

```markdown
# Contract Review: Acme SaaS Agreement

**Document Type:** SaaS Subscription Agreement
**Your Position:** Customer
**Risk Level:** 🟡 Medium

## Key Terms

| Term | Value | Location |
|------|-------|----------|
| Liability Cap | 3 months' fees | Section 10.2 |
| Auto-Renewal | 60-day notice | Section 8.2 |

## Risk Analysis

### 🔴 Critical

**Limitation of Liability** (Section 10.2)
> "Liability shall not exceed fees paid in the preceding three (3) months"

- **Issue:** 3-month cap is below market standard (12 months typical)
- **Redline:** Change "three (3) months" → "twelve (12) months"
- **Fallback:** Accept 6 months as compromise

### 🟢 Reviewed & Acceptable

| Category | Notes |
|----------|-------|
| Data Ownership | ✓ Customer owns all data |
| Governing Law | ✓ Delaware - standard |
```

---

## Features

### Position-Aware Review
Tell it which party you are (customer, vendor, buyer, seller) — the skill adjusts what it flags as risky.

### CUAD 41 Risk Categories
Based on the [Contract Understanding Atticus Dataset](https://github.com/TheAtticusProject/cuad) (NeurIPS 2021):
- Termination provisions
- Liability caps and carve-outs
- Indemnification scope
- Assignment and change of control
- IP and confidentiality
- Non-compete enforceability
- And 35 more...

### Jurisdiction Awareness
Flags when governing law affects enforceability:
- Non-competes void in CA/ND/OK
- Delaware vs NY vs CA implications

### M&A Support
Special handling for acquisition agreements:
- Earnout mechanics and measurement
- Rep & warranty survival periods
- Working capital adjustments
- Escrow/holdback provisions

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
Review this NDA for red flags

Analyze the indemnification in this MSA - I'm the vendor

What are the termination provisions? I'm the customer.

Review this acquisition agreement - I'm the seller
```

---

## Optional: Zuva Integration

For automated clause extraction using 1,354+ pre-trained legal fields:

```bash
pip install git+https://github.com/zuvaai/zdai-python.git
python -m zdai --set-token YOUR_TOKEN
python -m zdai --set-url https://us.app.zuva.ai/api/v2
python -m zdai --test connection
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
