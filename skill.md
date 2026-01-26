---
name: contract-review
description: Review contracts for risks, extract key terms, and suggest redlines using validated legal AI patterns
version: 1.0.0
author: Chris Sheehan
---

# Contract Review Skill

A Claude Code skill for reviewing legal contracts. Built on validated patterns from Anthropic's legal team, the CUAD dataset (NeurIPS 2021), and ContractEval benchmarks.

## When to Activate

- User mentions "review contract", "analyze agreement", "check this contract"
- User uploads or references a PDF/DOCX legal document
- User asks about specific clauses, risks, or terms in an agreement

## Capabilities

1. **Risk Identification** - Flag problematic clauses using CUAD's 41 risk categories
2. **Key Term Extraction** - Extract parties, dates, amounts, obligations in structured format
3. **Clause Analysis** - Identify and explain specific provision types
4. **Redline Suggestions** - Propose alternative language for one-sided terms
5. **Summary Generation** - Create executive summaries with risk ratings

## Output Format

Always use XML-structured output for parseability:

```xml
<contract_review>
  <summary>
    <document_type>...</document_type>
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
    <risk category="..." severity="Low|Medium|High">
      <clause>Exact quote from contract</clause>
      <issue>Why this is problematic</issue>
      <recommendation>Suggested action or alternative language</recommendation>
    </risk>
  </risk_analysis>

  <missing_provisions>
    <provision name="...">
      <importance>Why this matters</importance>
      <suggested_language>Optional template language</suggested_language>
    </provision>
  </missing_provisions>
</contract_review>
```

## Risk Categories (CUAD-based)

Check contracts for these 41 clause types, flagging unusual or one-sided terms:

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
- Price Restrictions
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
- License Grant (scope, exclusivity)
- Non-Compete
- Non-Solicitation of Employees
- Non-Solicitation of Customers
- Exclusivity
- Non-Disparagement
- Confidentiality Duration

### Dispute Resolution
- Governing Law
- Jurisdiction
- Arbitration vs Litigation
- Jury Trial Waiver

### Special Provisions
- ROFR / ROFO / ROFN (Rights of First Refusal/Offer/Negotiation)
- Revenue/Profit Sharing
- Joint IP Ownership
- Source Code Escrow

## Review Process

1. **Identify Document Type**
   - What kind of agreement is this? (NDA, MSA, SaaS, Employment, M&A, etc.)
   - This determines which risk categories are most relevant

2. **Extract Key Metadata**
   - Parties and their roles
   - Dates (effective, expiration, renewal)
   - Term and notice periods
   - Governing law and jurisdiction

3. **Scan for High-Risk Clauses**
   - Work through relevant CUAD categories
   - Quote exact language when flagging issues
   - Note section/page numbers

4. **Assess Balance**
   - Is the agreement mutual or one-sided?
   - Which party bears more risk?
   - Are there unusual or non-standard terms?

5. **Check for Missing Provisions**
   - What's notably absent?
   - Standard protections that should be present?

6. **Provide Actionable Output**
   - Clear risk ratings
   - Specific redline suggestions where appropriate
   - Prioritized list of items to negotiate

## M&A Specific Considerations

For acquisition-related agreements, also check:
- Earnout mechanics and measurement
- Escrow and holdback provisions
- Rep & warranty scope and survival periods
- Basket and cap structures
- Sandbagging provisions
- Non-compete scope and duration
- Employee retention terms
- Closing conditions

## Guardrails

- **No legal advice**: Always note this is for informational purposes and recommend attorney review for material terms
- **Flag tax implications**: Note but don't opine on tax treatment
- **Jurisdiction awareness**: Note when provisions may vary by jurisdiction
- **Confidence levels**: When uncertain about interpretation, say so
- **No hallucination**: Only reference text actually present in the document

## Example Usage

User: "Review this NDA for any red flags"

Response format:
1. Brief summary of the document type and parties
2. Key terms table
3. Risk analysis with specific clause quotes
4. Missing provisions if any
5. Recommended actions prioritized by importance

## Integration with Zuva

If Zuva API is configured, this skill can use it for:
- Automated clause extraction (1,354 pre-trained fields)
- Document classification
- High-confidence field identification

Check Zuva availability:
```bash
source .venv/bin/activate && python -m zdai --test connection
```
