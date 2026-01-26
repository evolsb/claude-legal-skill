# NDA Review Example

## Prompt

```
Review this NDA for red flags. I'm the receiving party.
```

## Example Output

```markdown
# Contract Review: Mutual Non-Disclosure Agreement

**Document Type:** Mutual NDA
**Your Position:** Receiving Party
**Counterparty:** Acme Corp
**Risk Level:** 🟡 Medium

## Executive Summary

Mutual NDA with some one-sided provisions favoring the disclosing party. The 5-year
confidentiality period and hidden non-solicitation clause need attention.

---

## Key Terms

| Term | Value | Location |
|------|-------|----------|
| Term | 2 years | Section 5 |
| Confidentiality Survival | 5 years post-termination | Section 5.2 |
| Governing Law | New York | Section 8 |
| Definition Scope | "All information disclosed" — very broad | Section 1.1 |

---

## Risk Analysis

### 🟡 Important

**Confidentiality Duration** (Section 5.2)
> "The obligations of confidentiality shall survive for five (5) years following termination"

- **Issue:** 5-year survival is longer than standard (2-3 years for general business info)
- **Redline:** Change "five (5) years" → "three (3) years"
- **Alternative:** Add carve-out: "except for trade secrets, which shall survive indefinitely"

---

### 🔴 Critical

**Non-Solicitation** (Section 6.3)
> "Receiving Party shall not solicit any employees of Disclosing Party for 12 months"

- **Issue:** Non-solicitation is unusual in an NDA; may be unenforceable in some states
- **Redline:** Delete entire Section 6.3
- **Fallback:** Limit to "shall not directly solicit employees with whom Receiving Party had contact during discussions"

---

### 🟡 Important

**Broad Definition** (Section 1.1)
> "Confidential Information means all information disclosed by either party, whether oral, written, or electronic"

- **Issue:** "All information" without carve-outs could include publicly available info
- **Redline:** Add standard exceptions: "(a) publicly available; (b) already known; (c) independently developed; (d) received from third party"

---

## Reviewed & Acceptable

| Category | Status | Notes |
|----------|--------|-------|
| Mutual Obligations | ✓ | Both parties bound equally |
| Return/Destruction | ✓ | Standard 30-day requirement |
| Injunctive Relief | ✓ | Reasonable — acknowledges irreparable harm |
| Governing Law | ✓ | New York — standard for commercial |

---

## Missing Provisions

| Provision | Priority | Suggested Language |
|-----------|----------|-------------------|
| Residuals Clause | Important | "Nothing restricts use of Residuals (information retained in unaided memory)" |
| Compelled Disclosure | Important | "May disclose if legally required, with prompt notice to other party" |

---

*This review is for informational purposes only.*
```

## Common NDA Red Flags

1. **Overly broad definitions** — "All information" without exceptions
2. **One-way obligations** — Only one party bound
3. **Long survival** — 5+ years for non-trade-secret info
4. **Hidden restrictive covenants** — Non-compete, non-solicit buried in NDA
5. **No residuals clause** — Can't use general knowledge gained
6. **Pre-agreed injunctive relief** — Specific performance without bond
7. **Broad assignability** — Other party can assign without consent
