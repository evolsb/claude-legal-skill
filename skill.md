---
name: contract-review
description: Review contracts for risks, extract key terms, and suggest redlines using validated legal AI patterns
version: 2.0.0
---

# Contract Review Skill

Review legal contracts for risks, extract key terms, and suggest redlines. Built on validated patterns from the CUAD dataset (NeurIPS 2021), ContractEval benchmarks, and LegalBench.

## When to Activate

- User mentions "review contract", "analyze agreement", "check this contract"
- User uploads or references a PDF/DOCX legal document
- User asks about specific clauses, risks, or terms in an agreement

## Capabilities

1. **Risk Identification** - Flag problematic clauses using CUAD's 41 risk categories
2. **Key Term Extraction** - Extract parties, dates, amounts, obligations
3. **Clause Analysis** - Identify and explain specific provision types
4. **Redline Suggestions** - Propose specific alternative contract language
5. **Gap Analysis** - Identify missing standard provisions

## First Step: Identify User's Position

Before reviewing, determine the user's role:

**Ask if unclear:** "Which party are you in this agreement? (e.g., customer, vendor, buyer, seller, licensor, licensee)"

This fundamentally affects what's "favorable" vs "unfavorable":
- Customer reviewing vendor's SaaS agreement → flag vendor-favorable terms
- Vendor reviewing their own template → flag customer-favorable terms
- Buyer in M&A → flag seller-favorable terms
- Seller in M&A → flag buyer-favorable terms

## Output Format

Use XML-structured output for parseability. Follow these rules:

1. **Character escaping**: Use `&lt;` `&gt;` `&amp;` `&quot;` for special chars in clause text
2. **Empty sections**: Omit section if nothing to report
3. **Enums are strict**: `risk_level` and `severity` must be exactly `Low`, `Medium`, or `High`
4. **Location format**: "Section X.Y, Page Z" or "Section X.Y" (page optional)

```xml
<contract_review>
  <summary>
    <document_type>...</document_type>
    <user_position>Customer|Vendor|Buyer|Seller|etc.</user_position>
    <parties>...</parties>
    <effective_date>...</effective_date>
    <risk_level>Low|Medium|High</risk_level>
    <executive_summary>2-3 sentence overview</executive_summary>
  </summary>

  <key_terms>
    <term name="...">
      <value>...</value>
      <location>Section X, Page Y</location>
    </term>
  </key_terms>

  <risk_analysis>
    <risk category="..." severity="Low|Medium|High" priority="Critical|Important|Minor">
      <clause>Exact quote from contract</clause>
      <issue>Why this is problematic for user's position</issue>
      <recommendation>
        <position>What to negotiate for</position>
        <redline>
          <original>Current contract language</original>
          <proposed>Suggested replacement language</proposed>
        </redline>
        <fallback>Compromise position if primary ask rejected</fallback>
      </recommendation>
    </risk>
  </risk_analysis>

  <reviewed_acceptable>
    <!-- Categories reviewed and found acceptable -->
    <category name="..." note="Brief reason why acceptable"/>
  </reviewed_acceptable>

  <missing_provisions>
    <provision name="..." priority="Critical|Important|Minor">
      <importance>Why this matters</importance>
      <suggested_language>
        <insert_location>After Section X</insert_location>
        <new_section>Full contract language to add</new_section>
      </suggested_language>
    </provision>
  </missing_provisions>

  <internal_consistency>
    <!-- Cross-reference errors, definition gaps, etc. -->
    <issue type="cross_reference|definition|conflict">
      <description>...</description>
      <location>...</location>
      <fix>...</fix>
    </issue>
  </internal_consistency>

  <disclaimer>This review is for informational purposes only and does not constitute legal advice. Material terms should be reviewed by qualified legal counsel.</disclaimer>
</contract_review>
```

## Risk Categories (Complete CUAD 41)

Check contracts for these clause types, flagging terms unfavorable to user's position:

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
- Indemnification (scope, who's covered)
- Insurance Requirements
- Warranty Duration

### IP & Confidentiality
- IP Ownership Assignment
- License Grant (scope, exclusivity)
- Affiliate License - Licensor
- Affiliate License - Licensee
- Covenant Not To Sue
- Non-Compete
- Non-Solicitation of Employees
- Non-Solicitation of Customers
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
- What kind of agreement? (NDA, MSA, SaaS, Employment, M&A, License, etc.)
- Which party is the user?
- What's the business relationship and power dynamic?

### Step 2: Extract Key Metadata
- Parties and their roles
- Dates (effective, expiration, renewal)
- Term and notice periods
- Governing law and jurisdiction
- Key financial terms

### Step 3: Analyze Risk (with reasoning)

For each potential risk, think through:
- What does this clause actually say? (quote exactly)
- Is this mutual or one-sided?
- What's market standard for this agreement type?
- What's the realistic business impact?
- Does this create material exposure for the user?

**Only flag if:**
- The term is genuinely unusual for this contract type, OR
- It creates material business risk for user's position, OR
- It's significantly one-sided without justification

### Step 4: Check Internal Consistency
- Are all cross-references valid?
- Are all capitalized terms defined?
- Do survival clauses capture necessary sections?
- Do exhibits match body references?

### Step 5: Identify Missing Provisions
- What standard protections are absent?
- What should be present for this contract type?

### Step 6: Prioritize Output
- **Critical**: Must address before signing
- **Important**: Should negotiate if possible
- **Minor**: Nice to have, lower priority

## Risk Severity Guidelines

Severity depends on contract type and user's position:

| Provision | Low | Medium | High |
|-----------|-----|--------|------|
| **Liability cap** | 12+ months fees | 6-11 months | <6 months or uncapped |
| **Non-compete** | 1-2 years, limited scope | 3-4 years | 5+ years or overly broad |
| **Auto-renewal notice** | 90+ days | 60-89 days | <60 days |
| **Termination for convenience** | Mutual with 90+ days | One-sided or <60 days | Immediate or no cure |
| **Indemnification** | Mutual, capped | Asymmetric | Uncapped, broad scope |

## Jurisdiction-Specific Notes

Flag when jurisdiction significantly affects enforceability:

**Non-Competes:**
- California, North Dakota, Oklahoma: Generally void
- Other states: Reasonableness test (duration, geography, scope)

**Choice of Law:**
- Delaware: Corp-friendly, sophisticated commercial law
- New York: Common for financial agreements
- California: Employee/consumer-friendly

**Arbitration:**
- Employment/consumer contracts: Class waivers may be unenforceable
- B2B commercial: Generally enforced

## M&A Specific Considerations

For acquisition agreements, additionally check:

### Deal Economics
- Earnout mechanics (measurement, buyer discretion, audit rights)
- Escrow/holdback (amount, duration, release conditions)
- Working capital adjustments
- Purchase price allocation

### Representations & Warranties
- Scope and knowledge qualifiers
- Survival periods (12-24 months typical, longer for fundamental)
- Materiality scrapes
- Sandbagging (pro vs anti)
- R&W insurance impact

### Deal Protection
- Material Adverse Change (MAC) definition
- Closing conditions
- Termination rights and break fees

### Post-Closing
- Non-compete (2-3 years typical)
- Employee matters
- Indemnification procedures

## Guardrails

- **No legal advice**: Note this is informational; recommend attorney review for material terms
- **No tax advice**: Flag tax implications but don't opine on treatment
- **Jurisdiction awareness**: Note when provisions vary by jurisdiction
- **Confidence levels**: Express uncertainty when interpretation is unclear
- **No hallucination**: Only reference text actually present in the document
- **Balanced assessment**: Include `<reviewed_acceptable>` section to show what's fine

## Integration with Zuva

If Zuva API is configured, use for automated clause extraction:

**When to use Zuva:**
- Contracts >20 pages (faster extraction)
- Standard commercial agreements
- Batch processing

**When to skip:**
- Unusual contract types
- Focus on specific clauses only
- API unavailable

Check availability: `python -m zdai --test connection`
