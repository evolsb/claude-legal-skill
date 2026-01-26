---
name: contract-review
description: Review contracts for risks, extract key terms, and suggest redlines
version: 2.1.0
---

# Contract Review Skill

Review legal contracts for risks, extract key terms, and suggest redlines. Built on the CUAD dataset (41 risk categories), ContractEval benchmarks, and LegalBench.

## When to Activate

- User mentions "review contract", "analyze agreement", "check this contract"
- User uploads or references a PDF/DOCX legal document
- User asks about specific clauses, risks, or terms

## First Step: Identify User's Position

Before reviewing, determine the user's role:

**Ask if unclear:** "Which party are you? (customer, vendor, buyer, seller, licensor, licensee)"

This affects what's "risky":
- Customer reviewing vendor agreement → flag vendor-favorable terms
- Vendor reviewing own template → flag customer-favorable terms
- Buyer in M&A → flag seller-favorable terms
- Seller in M&A → flag buyer-favorable terms

## Output Format

Use **markdown** for readable, scannable output. Structure as follows:

---

### Example Output

```markdown
# Contract Review: [Document Name]

**Document Type:** SaaS Subscription Agreement
**Your Position:** Customer
**Counterparty:** Acme Software Inc.
**Risk Level:** 🟡 Medium

## Executive Summary

Standard vendor agreement with some one-sided terms. The 3-month liability cap and
asymmetric termination rights need attention. Data ownership is clear.

---

## Key Terms

| Term | Value | Location |
|------|-------|----------|
| Initial Term | 12 months | Section 8.1 |
| Auto-Renewal | 12-month periods, 60-day notice | Section 8.2 |
| Liability Cap | 3 months' fees | Section 10.2 |
| Governing Law | Delaware | Section 12.1 |

---

## Risk Analysis

### 🔴 Critical

**Limitation of Liability** (Section 10.2)
> "Liability shall not exceed fees paid in the preceding three (3) months"

- **Issue:** 3-month cap is below market standard (typically 12 months)
- **Risk:** For $120K annual contract, liability capped at $30K
- **Recommendation:** Request 12-month cap
- **Redline:** Change "three (3) months" → "twelve (12) months"
- **Fallback:** Accept 6 months as compromise

---

### 🟡 Important

**Termination for Convenience** (Section 8.5)
> "Vendor may terminate for any reason upon 30 days notice"

- **Issue:** One-sided; customer lacks equivalent right
- **Recommendation:** Add mutual termination or extend notice to 90 days
- **Redline:** Add "Either party may terminate..." or change to "90 days"

---

### 🟢 Reviewed & Acceptable

| Category | Status | Notes |
|----------|--------|-------|
| Data Ownership | ✓ | Customer owns all customer data |
| IP Rights | ✓ | Clear separation, no broad assignment |
| Confidentiality | ✓ | Mutual, 3-year term, standard exceptions |
| Governing Law | ✓ | Delaware - neutral for commercial |

---

## Missing Provisions

| Provision | Priority | Why It Matters |
|-----------|----------|----------------|
| Data Export Rights | Critical | No guaranteed way to get data out on termination |
| SLA Credits | Important | 99.9% uptime stated but no remedy for breach |
| Price Increase Cap | Important | Renewal pricing uncapped |

**Suggested language for Data Export:**
> "Upon termination, Vendor shall make Customer Data available for export in CSV or JSON format for 90 days at no additional charge."

---

## Internal Consistency Issues

- ⚠️ Section 5.2 references "Exhibit C" but no Exhibit C exists
- ⚠️ "Confidential Information" defined in Section 3.1 but used lowercase in Section 7

---

*This review is for informational purposes only. Material terms should be reviewed by qualified legal counsel.*
```

---

## Risk Categories (CUAD 41)

Check for these clause types, flagging terms unfavorable to user's position:

### Document Basics
- Document Name and Type
- Parties (legal names, roles)
- Agreement Date / Effective Date
- Expiration Date
- Renewal Terms

### Term & Termination
- Contract Term / Duration
- Termination for Convenience
- Termination for Cause
- Post-Termination Services
- Survival Clauses

### Assignment & Control
- Anti-Assignment Clause
- Change of Control
- Consent Requirements

### Financial Terms
- Payment Terms
- Price Restrictions / Adjustments
- Most Favored Nation (MFN)
- Minimum Commitment
- Volume Restrictions
- Audit Rights

### Liability & Risk
- Limitation of Liability
- Cap on Liability
- Uncapped Liability Carve-outs
- Indemnification
- Insurance Requirements
- Warranty Duration

### IP & Confidentiality
- IP Ownership Assignment
- License Grant
- Affiliate License - Licensor/Licensee
- Covenant Not To Sue
- Non-Compete
- Non-Solicitation (Employees/Customers)
- Competitive Restriction Exception
- Exclusivity
- Non-Disparagement
- Confidentiality Duration
- Third Party Beneficiary

### Dispute Resolution
- Governing Law
- Jurisdiction / Venue
- Arbitration vs Litigation
- Jury Trial Waiver

### Special Provisions
- ROFR / ROFO / ROFN
- Revenue/Profit Sharing
- Joint IP Ownership
- Source Code Escrow
- Irrevocable or Perpetual License

## Review Process

### Step 1: Identify Document Type & User Position
- What kind of agreement? (NDA, MSA, SaaS, Employment, M&A, etc.)
- Which party is the user?
- What's the power dynamic?

### Step 2: Extract Key Metadata
- Parties and roles
- Key dates (effective, expiration, renewal)
- Term and notice periods
- Governing law
- Key financial terms

### Step 3: Analyze Risk

For each potential issue:
1. Quote the exact clause
2. Explain why it's problematic for user's position
3. Compare to market standard
4. Provide specific redline language
5. Suggest fallback position

**Only flag if:**
- Genuinely unusual for this contract type, OR
- Creates material business risk, OR
- Significantly one-sided without justification

### Step 4: Check Internal Consistency
- Cross-references valid?
- All capitalized terms defined?
- Survival clauses complete?
- Exhibits match body?

### Step 5: Identify Missing Provisions
- Standard protections absent?
- Suggest specific language to add

### Step 6: Prioritize Output
- 🔴 **Critical**: Must fix before signing
- 🟡 **Important**: Should negotiate
- 🟢 **Minor**: Nice to have

## Risk Severity Guidelines

| Provision | Low | Medium | High |
|-----------|-----|--------|------|
| Liability cap | 12+ months | 6-11 months | <6 months |
| Non-compete | 1-2 years | 3-4 years | 5+ years |
| Auto-renewal notice | 90+ days | 60-89 days | <60 days |
| Termination notice | Mutual, 90+ days | One-sided or <60 days | Immediate |
| Indemnification | Mutual, capped | Asymmetric | Uncapped |

## Jurisdiction Notes

**Non-Competes:**
- California, North Dakota, Oklahoma: Generally void
- Other states: Reasonableness test applies

**Choice of Law:**
- Delaware: Corp-friendly
- New York: Financial agreements
- California: Employee-friendly

## M&A Considerations

For acquisition agreements, also check:
- Earnout mechanics (measurement, discretion, audit rights)
- Escrow/holdback (amount, duration, release)
- Working capital adjustments
- Rep & warranty survival (12-24 months typical)
- Materiality scrapes, sandbagging
- MAC definition
- Non-compete scope (2-3 years typical)

## Guardrails

- **Not legal advice**: Recommend attorney review for material terms
- **Not tax advice**: Flag but don't opine
- **Jurisdiction matters**: Note when enforceability varies
- **Express uncertainty**: Say when interpretation is unclear
- **No hallucination**: Only reference text actually in document
- **Show what's acceptable**: Include "Reviewed & Acceptable" section

## Zuva Integration (Optional)

For automated clause extraction with 1,354 pre-trained fields:
```bash
python -m zdai --test connection
```
