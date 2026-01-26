# Balanced Agreement Example — Low Risk

Shows what a well-balanced contract looks like and how to report acceptable terms.

## Prompt

```
I'm the customer. Review this SaaS agreement.
```

## Example Output

```markdown
# Contract Review: Acme Cloud Platform Agreement

**Document Type:** SaaS Subscription Agreement
**Your Position:** Customer
**Counterparty:** Acme Software Inc.
**Risk Level:** 🟢 Low

## Executive Summary

Well-balanced commercial agreement with mutual protections. Terms are within market
standards for enterprise SaaS. Minor points exist but no critical issues.

---

## Key Terms

| Term | Value | Location |
|------|-------|----------|
| Initial Term | 12 months | Section 8.1 |
| Auto-Renewal | 12-month periods, 90-day notice | Section 8.2 |
| Liability Cap | 12 months' fees | Section 10.2 |
| SLA | 99.9% uptime, 10% credits | Exhibit B |
| Governing Law | Delaware | Section 12.1 |

---

## Risk Analysis

### 🟢 Minor

**Price Adjustments** (Section 4.5)
> "Vendor may adjust pricing upon renewal with 60 days notice, not to exceed 5% annually"

- **Issue:** Price increases capped but could compound over time
- **Assessment:** 5% cap is reasonable; this is low risk
- **Optional ask:** CPI-linked cap instead of flat 5%

---

## Reviewed & Acceptable

| Category | Status | Notes |
|----------|--------|-------|
| Limitation of Liability | ✓ | 12-month cap — market standard |
| Indemnification | ✓ | Mutual with reasonable scope |
| Data Ownership | ✓ | Customer owns all customer data |
| Data Export | ✓ | 90-day export period, standard formats |
| Termination | ✓ | Mutual for convenience with 90 days notice |
| Auto-Renewal | ✓ | 90-day notice period is adequate |
| SLA | ✓ | 99.9% with meaningful credits |
| Governing Law | ✓ | Delaware — neutral for commercial |
| IP Rights | ✓ | Customer retains all IP in data/configs |
| Confidentiality | ✓ | Mutual, 3-year term, standard exceptions |
| Assignment | ✓ | Consent required, CoC exception |

---

## Missing Provisions

None critical. Agreement is comprehensive.

---

*This review is for informational purposes only. Material terms should be reviewed by qualified legal counsel.*
```

## Key Lesson

Not every contract needs extensive redlines. When terms are balanced:

1. **Show what's acceptable** — the "Reviewed & Acceptable" table builds confidence
2. **Don't over-flag** — minor issues stay 🟢, not everything is 🔴
3. **Provide context** — explain why terms are fine
4. **Save negotiation capital** — push back only on truly problematic terms

## Signs of a Balanced Agreement

- Mutual obligations (indemnification, confidentiality, termination)
- 12-month liability cap (market standard for SaaS)
- 60-90 day notice periods
- Clear data ownership and export rights
- Meaningful SLA with actual remedies
- Price increase caps
- Neither party has dramatically more power
