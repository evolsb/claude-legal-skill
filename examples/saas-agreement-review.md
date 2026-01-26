# SaaS Agreement Review Example

## Prompt

```
Review this SaaS agreement. I'm the customer.
```

## Example Output

```markdown
# Contract Review: CloudCo Platform Subscription

**Document Type:** SaaS Subscription Agreement
**Your Position:** Customer
**Counterparty:** CloudCo Inc.
**Risk Level:** 🔴 High

## Executive Summary

One-sided vendor agreement with significant customer risk. The 3-month liability cap,
missing uptime SLA, and uncapped price increases need immediate attention.

---

## Key Terms

| Term | Value | Location |
|------|-------|----------|
| Initial Term | 36 months | Section 8.1 |
| Annual Fee | $120,000/year | Exhibit A |
| Liability Cap | 3 months' fees ($30,000) | Section 9.2 |
| Auto-Renewal | 30-day notice to cancel | Section 8.2 |
| Uptime SLA | None specified | — |

---

## Risk Analysis

### 🔴 Critical

**Limitation of Liability** (Section 9.2)
> "In no event shall Vendor's liability exceed the fees paid in the three (3) months preceding the claim"

- **Issue:** 3-month cap is unusually low; market standard is 12 months
- **Risk:** For $120K/year contract, liability capped at $30K
- **Redline:** Change "three (3) months" → "twelve (12) months"
- **Fallback:** Accept 6 months minimum

---

### 🔴 Critical

**No Service Level Agreement**

- **Issue:** No uptime commitment or service credits for outages
- **Risk:** Vendor has no contractual obligation to maintain service
- **Add:** SLA exhibit with 99.9% uptime and credit structure:

> **Suggested SLA language:**
> "Vendor commits to 99.9% monthly uptime. For each full percentage point below 99.9%, Customer receives credit of 10% of monthly fees, up to 25%. Uptime below 95% in any month is material breach."

---

### 🟡 Important

**Auto-Renewal Notice** (Section 8.2)
> "Agreement auto-renews unless either party provides notice 30 days prior"

- **Issue:** 30 days is very tight for a 36-month, $120K contract
- **Redline:** Change "30 days" → "90 days"

---

### 🟡 Important

**Price Increases** (Section 4.3)
> "Vendor may adjust pricing upon any renewal with 30 days notice"

- **Issue:** No cap on increases; could double pricing at renewal
- **Redline:** Add "not to exceed 5% annually" or "CPI + 3%"

---

## Reviewed & Acceptable

| Category | Status | Notes |
|----------|--------|-------|
| Data Ownership | ✓ | Customer owns customer data |
| Governing Law | ✓ | Delaware — standard |
| IP in Configs | ✓ | Customer retains |

---

## Missing Provisions

| Provision | Priority | Why It Matters |
|-----------|----------|----------------|
| Data Export | Critical | No way to get data out on termination |
| Security Standards | Critical | No SOC 2 or security commitments |
| Subprocessor Notice | Important | No visibility into third-party data access |
| Transition Assistance | Important | No help migrating away |

**Suggested Data Export language:**
> "Upon termination, Vendor shall make Customer Data available for export in JSON or CSV format for 90 days at no charge."

---

## Internal Consistency

- ⚠️ "Services" defined in Section 1.2 but Exhibit A references "Platform Services"

---

*This review is for informational purposes only.*
```

## Vendor-Favorable Terms to Watch

1. **Liability cap tied to recent fees** — 3 months on annual contracts is very low
2. **No uptime SLA** — Even 99.9% with credits is better than nothing
3. **Short renewal notice** — 30 days is tight for large contracts
4. **Uncapped price increases** — Always negotiate a cap
5. **Broad vendor termination** — Customer should have this, not just vendor
6. **One-sided indemnification** — Should be mutual
7. **No data export rights** — Critical for avoiding vendor lock-in
