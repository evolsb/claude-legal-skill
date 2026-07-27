---
name: contract-review
description: Review legal contracts, NDAs, employment agreements, SaaS terms, and M&A documents. Identifies unfavorable terms, suggests redlines, and compares to market standards. Also generates Terms & Conditions, Privacy Policy, Anti-Spam Policy, Cookie Policy, AUP, DMCA Policy, and other app legal documents from user intake. Use for contract analysis, due diligence, negotiation prep, or drafting legal docs for apps.
version: 4.0.0
---

# Contract Review & Legal Document Generation Skill

Two modes:
1. **Review Mode** — analyze existing contracts for risks, redlines, market benchmarks
2. **Generate Mode** — draft legal documents for apps from scratch via intake Q&A

Built on CUAD dataset (41 risk categories), ContractEval benchmarks, and LegalBench.

## When to Activate

**Review Mode:**
- User mentions "review contract", "analyze agreement", "check this contract"
- User uploads or references a PDF/DOCX legal document
- User asks about specific clauses, risks, or terms

**Generate Mode:**
- User says "generate", "create", "draft", "write" + any policy or agreement
- User says "I need terms and conditions", "I need a privacy policy", "help me draft"
- User building an app and needs legal documents
- User asks "what policies do I need for my app"

---

## Step 1: Pre-Review Checklist

Before analyzing content, verify document completeness:

- [ ] **Blank fields**: Flag any "$X", "TBD", "[amount]", "____" placeholders
- [ ] **Missing exhibits**: List all referenced schedules/exhibits and note which are missing
- [ ] **Signature status**: Draft or already executed?
- [ ] **All pages present**: Check for truncation or missing sections

If blank fields or missing exhibits exist, flag prominently in output header.

---

## Step 2: Identify Document Type & User Position

**Ask if unclear:** "Which party are you? (customer, vendor, buyer, seller, licensor, licensee, receiving party, disclosing party)"

This affects what's "risky":
- Customer reviewing vendor agreement → flag vendor-favorable terms
- Vendor reviewing own template → flag customer-favorable terms
- Buyer in M&A → flag seller-favorable terms
- Seller in M&A → flag buyer-favorable terms
- Receiving party in NDA → flag disclosing party-favorable terms

**Assess power dynamic:**
- Startup vs. large enterprise? (limited negotiating leverage)
- Standard form vs. negotiated? (some terms non-negotiable)
- Regulated industry? (some terms legally required)

---

## Output Format

Use **markdown** for readable, scannable output. Do NOT use XML tags.

---

### Example Output

```markdown
# Contract Review: [Document Name]

**Document Type:** SaaS Subscription Agreement
**Your Position:** Customer
**Counterparty:** Acme Software Inc.
**Risk Level:** 🟡 Medium
**Document Status:** Draft / Executed on [date]

## ⚠️ Pre-Signing Alerts

- **Blank field:** Fee amount in Section 4.1 is "$____"
- **Missing exhibit:** Exhibit B (SLA) referenced but not attached

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

## Red Flags (Quick Scan)

| Flag | Found | Location |
|------|-------|----------|
| Liability cap < 6 months | ⚠️ Yes | Section 10.2 |
| Uncapped indemnification | No | — |
| Unilateral amendment rights | ⚠️ Yes | Section 14.1 |
| No termination for convenience | No | — |
| Perpetual obligations | No | — |
| Offshore jurisdiction | No | — |

---

## Risk Analysis

### 🔴 Critical

**Limitation of Liability** (Section 10.2)
> "Liability shall not exceed fees paid in the preceding three (3) months"

- **Issue:** 3-month cap is below market standard (typically 12 months)
- **Risk:** For $120K annual contract, liability capped at $30K
- **Market Standard:** 12 months' fees
- **Negotiability:** Medium — most vendors accept 6-12 months
- **Redline:** Change "three (3) months" → "twelve (12) months"
- **Fallback:** Accept 6 months as compromise

---

### 🟡 Important

**Termination for Convenience** (Section 8.5)
> "Vendor may terminate for any reason upon 30 days notice"

- **Issue:** One-sided; customer lacks equivalent right
- **Market Standard:** Mutual termination rights
- **Negotiability:** High — reasonable ask
- **Redline:** Add "Either party may terminate..." or change to "90 days"

---

### 🟢 Reviewed & Acceptable

| Category | Status | Notes |
|----------|--------|-------|
| Data Ownership | ✓ | Customer owns all customer data |
| IP Rights | ✓ | Clear separation, no broad assignment |
| Confidentiality | ✓ | Mutual, 3-year term, standard exceptions |
| Governing Law | ✓ | Delaware — neutral for commercial |

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

## Negotiation Priority

| # | Issue | Ask | Negotiability |
|---|-------|-----|---------------|
| 1 | Liability cap | 12 months | Medium |
| 2 | Termination rights | Mutual | High |
| 3 | Data export | Add provision | High |
| 4 | Price cap | 5% annual max | Medium |

---

*This review is for informational purposes only. Material terms should be reviewed by qualified legal counsel.*
```

---

## Red Flags Quick Scan

Check these danger signs FIRST before deep analysis:

| Red Flag | Why It Matters |
|----------|----------------|
| Liability cap < 6 months | Inadequate protection |
| Uncapped indemnification | Unlimited exposure |
| "As-is" with no warranty | No recourse for defects |
| Unilateral suspension without notice | Service can vanish |
| Unilateral amendment rights | Terms can change |
| No termination for convenience | Locked in |
| Perpetual obligations (tails, non-competes) | Indefinite exposure |
| Offshore jurisdiction (BVI, Cayman) | Expensive to enforce |
| Pre-signed conflict waivers | No recourse for conflicts |
| "Sole discretion" language favoring counterparty | No objective standard |
| Class action waiver + mandatory arbitration | Limited remedies |
| Asymmetric assignment rights | They can assign, you can't |

---

## Document Type Checklists

### NDA Checklist

| Category | Check For |
|----------|-----------|
| Direction | One-way or mutual? |
| Definition scope | "All information" too broad? Standard exceptions? |
| Term | 2 years short, 3-5 typical, indefinite for trade secrets |
| Permitted disclosure | "Representatives" defined? Flow-down required? |
| Residuals clause | Can use general knowledge retained in memory? |
| Non-solicitation | Employees protected? |
| Standstill | Prevents hostile acquisition actions? |
| No-contact | Customers, suppliers, employees protected? |
| Return/destruction | Certification required? |
| Public announcement | Prohibits disclosure of discussions? |
| Compelled disclosure | Notice required? Time to seek protective order? |
| Injunctive relief | Pre-agreed specific performance? Bond waiver? |

### SaaS/MSA Checklist

| Category | Check For |
|----------|-----------|
| Liability cap | 12+ months = standard |
| Uptime SLA | 99.9% with credits = standard |
| Suspension rights | Unilateral? Notice required? |
| Data ownership | Customer owns customer data? |
| Data export | Format, duration, cost on termination? |
| Price increases | Capped? Notice period? |
| Auto-renewal notice | 90+ days = good, <60 = risk |
| Termination | Mutual for convenience? Cure period for cause? |
| Subprocessors | Notice of changes? Approval rights? |
| Insurance | Vendor carries E&O, cyber? |

### Payment/Merchant Agreement Checklist

| Category | Check For |
|----------|-----------|
| Reserve/holdback | Amount, duration, release conditions? |
| Chargeback liability | Capped? Fraud protection? |
| Network rules | Incorporated by reference? Access provided? |
| Auto-debit authority | Notice before debits? |
| Settlement timing | When do you receive funds? |
| Volume commitments | Realistic? Penalty for shortfall? |
| Suspension rights | Immediate or notice? |
| Termination tail | How long do obligations survive? |
| Audit rights | Frequency, notice, cost allocation? |
| PCI compliance | Who bears cost? |

### M&A Agreement Checklist

| Category | Check For |
|----------|-----------|
| Purchase price | Cash vs. stock vs. earnout mix? |
| Earnout mechanics | Measurement, discretion, audit rights, acceleration? |
| Escrow/holdback | Amount (10-15% typical), duration (12-18 mo), release? |
| Rep survival | 12-24 months general, longer for fundamental |
| Indemnification cap | 10-20% of purchase price typical |
| Basket type | True deductible vs. tipping? |
| Sandbagging | Pro-buyer or anti-sandbagging? |
| Non-compete | 2-3 years, geographic scope? |
| Working capital | Target, collar, true-up mechanism? |
| MAC definition | Carve-outs for market conditions? |
| Employment comp | Counted in purchase price or separate? |

### Finder/Broker Agreement Checklist

| Category | Check For |
|----------|-----------|
| Fee percentage | Specified or blank? |
| Fee calculation | What's included in deal value? Employment comp? |
| "Covered buyer" definition | How broad? Any prior relationship carve-out? |
| Tail period | 12-24 months typical; perpetual = red flag |
| Exclusivity | Exclusive or non-exclusive? |
| Minimum fee | Floor amount? |
| Joint representation | Consent required? Conflict waiver? |
| Escrow deduction | Auto-pay from proceeds? |
| Term/termination | Can you exit? |
| Broker status | BD registered if securities involved? |

---

## Risk Categories (CUAD 41 + Extensions)

### Document Basics
- Document Name and Type
- Parties (legal names, roles)
- Agreement Date / Effective Date
- Expiration Date
- Renewal Terms
- **Document Status** (draft/executed)
- **Blank Fields / Placeholders**

### Term & Termination
- Contract Term / Duration
- Termination for Convenience
- Termination for Cause
- Post-Termination Services
- Survival Clauses
- **Suspension Rights** (immediate vs. with notice)
- **Cure Periods**

### Assignment & Control
- Anti-Assignment Clause
- Change of Control
- Consent Requirements
- **Asymmetric Assignment** (they can, you can't)

### Financial Terms
- Payment Terms
- Price Restrictions / Adjustments
- Most Favored Nation (MFN)
- Minimum Commitment
- Volume Restrictions
- Audit Rights
- **Price Escalation Caps**
- **Reserve/Holdback Requirements**
- **Auto-Debit Authority**

### Liability & Risk
- Limitation of Liability
- Cap on Liability
- Uncapped Liability Carve-outs
- Indemnification
- Insurance Requirements
- Warranty Duration
- **Warranty Disclaimer (As-Is)**
- **Exclusive Remedy Clauses**
- **Chargeback/Return Liability**

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
- **Residuals Clause**
- **Feedback Ownership**

### Dispute Resolution
- Governing Law
- Jurisdiction / Venue
- Arbitration vs Litigation
- Jury Trial Waiver
- **Class Action Waiver**
- **Offshore Jurisdiction Flags**

### Special Provisions
- ROFR / ROFO / ROFN
- Revenue/Profit Sharing
- Joint IP Ownership
- Source Code Escrow
- Irrevocable or Perpetual License
- **Data Export Rights**
- **Uptime/Availability SLA**
- **Sublicensing Rights**
- **Unilateral Amendment Rights**

---

## Market Standard Benchmarks

| Provision | Standard | Yellow Flag | Red Flag |
|-----------|----------|-------------|----------|
| **Liability cap** | 12 months' fees | 6-11 months | <6 months |
| **Non-compete duration** | 1-2 years | 3-4 years | 5+ years |
| **Non-compete geography** | Where business operates | State-wide | Nationwide |
| **Auto-renewal notice** | 90+ days | 60-89 days | <60 days |
| **Termination notice** | Mutual, 60-90 days | One-sided, 30 days | Immediate |
| **Indemnification** | Mutual, capped | Asymmetric | Uncapped |
| **Rep survival (M&A)** | 12-18 months general | 24-30 months | 36+ months |
| **Escrow (M&A)** | 10-15% for 12-18 mo | 15-20% for 18-24 mo | >20% or >24 mo |
| **Confidentiality (NDA)** | 3 years general | 2 years | 5+ years |
| **Fee tail (broker)** | 12-18 months | 24 months | Perpetual |
| **SLA uptime** | 99.9% with credits | 99.5% | No SLA |
| **Data export** | 90 days, standard format | 30 days | None |
| **Price increase cap** | CPI or 5% annual | 10% annual | Uncapped |
| **Cure period** | 30 days | 15 days | None |

---

## Negotiability Guide

| Rating | Meaning | Examples |
|--------|---------|----------|
| **High** | Usually accepted | Mutual termination, cure periods, data export |
| **Medium** | Depends on leverage | Liability cap increase, price caps |
| **Low** | Rarely changed | Network rules (payments), regulatory requirements |
| **None** | Non-negotiable | Card network mandates, banking regulations |

**Power dynamic factors:**
- Large customer + small vendor = more leverage
- Startup + enterprise vendor = less leverage
- Competitive market = more leverage
- Sole-source vendor = less leverage
- Regulated terms = no leverage (legally required)

---

## Jurisdiction Notes

**Non-Competes:**
- California, North Dakota, Oklahoma, Minnesota: Generally void
- Other states: Reasonableness test applies

**Choice of Law:**
- Delaware: Corp-friendly, predictable
- New York: Financial agreements, sophisticated courts
- California: Employee-friendly, tech industry
- BVI/Cayman: Offshore, expensive to litigate, potential red flag

**Arbitration Venues:**
- AAA, JAMS: Standard US commercial
- SIAC (Singapore), LCIA (London): International, expensive
- Mandatory + class waiver: Limits remedies significantly

---

## Guardrails

- **Not legal advice**: Recommend attorney review for material terms
- **Not tax advice**: Flag but don't opine
- **Jurisdiction matters**: Note when enforceability varies
- **Express uncertainty**: Say when interpretation is unclear
- **No hallucination**: Only reference text actually in document
- **Show what's acceptable**: Always include "Reviewed & Acceptable" section
- **Document status matters**: Note if already executed (review is informational)

---

---

# PART 2: Legal Document Generation

Generate legal documents for apps via structured intake Q&A. Always collect required inputs before drafting. Output full markdown documents with `[PLACEHOLDER]` for any values not provided.

---

## Document Menu

When user asks "what do I need" or doesn't specify, present this list and ask which apply:

| # | Document | Required For |
|---|----------|-------------|
| 1 | **Terms & Conditions** | Any app with users |
| 2 | **Privacy Policy** | Any app collecting personal data (legally required in most jurisdictions) |
| 3 | **Anti-Spam Policy** | Any app sending email or SMS marketing |
| 4 | **Cookie Policy** | Any web app using cookies/tracking (required under GDPR/ePrivacy) |
| 5 | **Acceptable Use Policy (AUP)** | Apps with UGC, API access, or community features |
| 6 | **DMCA / Copyright Policy** | Platforms hosting user-uploaded content |
| 7 | **Refund & Cancellation Policy** | Paid apps, subscriptions, marketplaces |
| 8 | **Data Processing Agreement (DPA)** | B2B SaaS processing customer data under GDPR |

---

## Universal Intake (collect before ANY document)

Ask these first. Answers apply to all documents requested:

- [ ] **Company/App Name**
- [ ] **Website or App URL** (or "not launched yet")
- [ ] **App Type**: Web app / Mobile (iOS/Android) / Both / Marketplace / SaaS / API
- [ ] **Primary Contact Email** (shown in documents as legal/privacy contact)
- [ ] **Jurisdiction / User Base**: US only / EU users (GDPR required) / Canada (CASL) / Global
- [ ] **Effective Date** (use today's date if not specified)

If any of the above are missing, ask before generating. Do not generate with critical blanks unfilled.

---

## Document-Specific Intake

### Terms & Conditions Intake

- [ ] Free or paid service? (if paid: pricing model, subscription periods)
- [ ] User accounts / registration required?
- [ ] User-generated content (UGC)? (posts, uploads, reviews, comments)
- [ ] Age restriction? (under 13 → COPPA; under 18 → minor consent)
- [ ] Refund / cancellation policy? (yes/no, and what are the terms)
- [ ] Arbitration or litigation for disputes?
- [ ] Governing law state/country preference?
- [ ] Any specific prohibited uses to call out?

### Privacy Policy Intake

- [ ] What personal data is collected? (select all that apply)
  - Name, email, phone
  - Payment/billing info
  - Location data (GPS/IP)
  - Device identifiers
  - Usage/behavioral data
  - Health or sensitive data
- [ ] Analytics tools used? (Google Analytics, Mixpanel, Amplitude, etc.)
- [ ] Advertising / tracking pixels? (Meta Pixel, Google Ads, TikTok Pixel, etc.)
- [ ] Payment processor? (Stripe, PayPal, Braintree, etc.)
- [ ] Other third-party services / SDKs? (list them)
- [ ] Cookies or local storage used?
- [ ] EU users? → GDPR section required (legal basis, DPO, data subject rights)
- [ ] California users? → CCPA section required (opt-out, "Do Not Sell")
- [ ] Children under 13? → COPPA section required
- [ ] Data retention period? (e.g., "until account deletion", "3 years")
- [ ] Data deletion request process?

### Anti-Spam Policy Intake

- [ ] Email marketing / newsletters sent?
- [ ] SMS / text marketing sent?
- [ ] Opt-in mechanism: single opt-in / double opt-in / implied consent
- [ ] Unsubscribe mechanism: email link / SMS STOP / account settings
- [ ] Third-party email/SMS provider? (Mailchimp, SendGrid, Twilio, etc.)
- [ ] Transactional emails only, promotional emails, or both?
- [ ] US users → CAN-SPAM compliance
- [ ] Canada users → CASL compliance (express consent required)
- [ ] EU users → GDPR email rules (explicit consent, no soft opt-in)

### Cookie Policy Intake

- [ ] Strictly necessary cookies only, or also analytics / marketing / personalization?
- [ ] Cookie consent banner implemented?
- [ ] Cookie management platform? (OneTrust, Cookiebot, Osano, etc.)
- [ ] List of specific cookies / third-party services setting cookies (if known)

### Acceptable Use Policy Intake

- [ ] User-generated content platform?
- [ ] API access provided to third parties?
- [ ] Specific prohibited behaviors to enumerate? (scraping, bots, spam, hate speech, etc.)
- [ ] Consequences for violations? (warning / suspension / termination)

### DMCA / Copyright Policy Intake

- [ ] Designated DMCA agent name and contact
- [ ] Counter-notice process desired?
- [ ] Repeat infringer termination policy?

### Refund Policy Intake

- [ ] Subscription or one-time purchase?
- [ ] Refund window? (e.g., 14 days, 30 days, no refunds)
- [ ] Pro-rata refunds for cancellations mid-period?
- [ ] Digital goods / services (non-refundable by default in many jurisdictions)?
- [ ] App Store / Play Store purchases? (Apple/Google handle refunds separately)

---

## Generation Output Format

```markdown
# [Document Name]
**[Company/App Name]**
Effective Date: [DATE]
Last Updated: [DATE]

---

[Full document content with all sections]

---

*This document was generated for informational purposes only and does not constitute legal advice. Review with qualified legal counsel before publishing.*
```

Rules:
- Use `[PLACEHOLDER: description]` for values not provided in intake
- Mark jurisdiction-specific sections clearly: `> **GDPR (EU Users):**` / `> **CCPA (California Users):**`
- Every document ends with the legal disclaimer above
- If multiple documents requested in one session, generate them sequentially, reusing universal intake answers

---

## Generated Document Templates

### Terms & Conditions Template

```markdown
# Terms and Conditions
**[APP NAME]**
Effective Date: [DATE]
Last Updated: [DATE]

---

## 1. Acceptance of Terms

By accessing or using [APP NAME] ("Service"), you agree to be bound by these Terms and Conditions ("Terms"). If you do not agree, do not use the Service.

## 2. Description of Service

[APP NAME] provides [BRIEF DESCRIPTION OF SERVICE]. The Service is operated by [COMPANY NAME] ("Company", "we", "us").

## 3. Eligibility

You must be at least [13 / 18] years old to use the Service. [If under 18: Minors between 13–17 may use the Service only with parental consent.]

## 4. User Accounts

[IF ACCOUNTS REQUIRED:]
- You are responsible for maintaining the confidentiality of your account credentials.
- You are responsible for all activity under your account.
- Notify us immediately at [CONTACT EMAIL] if you suspect unauthorized access.
- We may suspend or terminate accounts that violate these Terms.

## 5. Acceptable Use

You agree not to:
- Violate any applicable laws or regulations
- Impersonate any person or entity
- Upload or transmit malicious code, spam, or harmful content
- Scrape, crawl, or data-mine the Service without written permission
- Circumvent any security or access controls
- [ANY APP-SPECIFIC PROHIBITED USES]

## 6. User-Generated Content

[IF UGC APPLIES:]
By submitting content to the Service, you grant [COMPANY NAME] a non-exclusive, worldwide, royalty-free license to use, display, reproduce, and distribute your content solely to operate the Service. You represent that you own or have rights to any content you submit. We reserve the right to remove content that violates these Terms.

[IF NO UGC:]
The Service does not permit users to submit or post content.

## 7. Intellectual Property

All content, features, and functionality of the Service — including but not limited to text, graphics, logos, and software — are owned by [COMPANY NAME] and protected by copyright, trademark, and other laws. You may not reproduce, distribute, or create derivative works without our written consent.

## 8. Payment Terms

[IF PAID SERVICE:]
- Fees are as described on our pricing page at [PRICING URL]
- Subscriptions automatically renew unless cancelled [X days] before the renewal date
- [REFUND POLICY: e.g., "We offer a [X]-day money-back guarantee for first-time subscribers"]
- All fees are in [USD / local currency] and exclusive of taxes

[IF FREE:]
The Service is currently provided free of charge. We reserve the right to introduce paid features with advance notice.

## 9. Disclaimers

THE SERVICE IS PROVIDED "AS IS" AND "AS AVAILABLE" WITHOUT WARRANTIES OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, OR NON-INFRINGEMENT. WE DO NOT WARRANT THAT THE SERVICE WILL BE UNINTERRUPTED, ERROR-FREE, OR FREE OF HARMFUL COMPONENTS.

## 10. Limitation of Liability

TO THE MAXIMUM EXTENT PERMITTED BY LAW, [COMPANY NAME] SHALL NOT BE LIABLE FOR ANY INDIRECT, INCIDENTAL, SPECIAL, CONSEQUENTIAL, OR PUNITIVE DAMAGES ARISING FROM YOUR USE OF THE SERVICE. OUR TOTAL LIABILITY SHALL NOT EXCEED [THE GREATER OF $100 OR THE AMOUNTS YOU PAID IN THE PRIOR 12 MONTHS].

## 11. Indemnification

You agree to indemnify and hold harmless [COMPANY NAME] and its officers, directors, employees, and agents from any claims, liabilities, damages, and expenses (including attorneys' fees) arising from your use of the Service or violation of these Terms.

## 12. Termination

We may suspend or terminate your access to the Service at any time for any reason, including violation of these Terms. Upon termination, your right to use the Service ceases immediately. Sections 9, 10, 11, and 13 survive termination.

## 13. Governing Law & Dispute Resolution

These Terms are governed by the laws of [STATE/COUNTRY], without regard to conflict-of-law principles.

[IF ARBITRATION:]
Any dispute arising from these Terms shall be resolved by binding arbitration under [AAA / JAMS] rules, conducted in [CITY, STATE]. You waive any right to a jury trial or class action.

[IF LITIGATION:]
You consent to the exclusive jurisdiction of the courts located in [CITY, STATE] for any disputes.

## 14. Changes to Terms

We may update these Terms at any time. We will notify you of material changes by [email / in-app notice / posting on website]. Continued use after the effective date constitutes acceptance.

## 15. Contact

[COMPANY NAME]
[ADDRESS if applicable]
Email: [CONTACT EMAIL]

---

*This document was generated for informational purposes only and does not constitute legal advice. Review with qualified legal counsel before publishing.*
```

---

### Privacy Policy Template

```markdown
# Privacy Policy
**[APP NAME]**
Effective Date: [DATE]
Last Updated: [DATE]

---

## 1. Introduction

[COMPANY NAME] ("we", "us", "our") operates [APP NAME] ("Service"). This Privacy Policy explains how we collect, use, disclose, and protect your personal information when you use the Service.

## 2. Information We Collect

**Information you provide directly:**
- [Name, email address] — when you create an account or contact us
- [Payment information] — processed by [PAYMENT PROCESSOR, e.g., Stripe]; we do not store card details
- [PLACEHOLDER: other data user provides]

**Information collected automatically:**
- Usage data (pages visited, features used, time spent)
- Device information (device type, OS, browser)
- IP address and approximate location
- [Cookies and tracking technologies — see Section 5]

**Information from third parties:**
- [PLACEHOLDER: e.g., social login providers, analytics partners]

## 3. How We Use Your Information

We use your information to:
- Provide, operate, and improve the Service
- Process transactions and send related notices
- Send administrative communications (account updates, security alerts)
- [IF MARKETING:] Send promotional communications (you may opt out at any time)
- Detect and prevent fraud, abuse, and security incidents
- Comply with legal obligations
- [PLACEHOLDER: other uses]

**Legal basis (GDPR):** We process your data based on contract performance, legitimate interests, consent (where required), and legal obligation.

## 4. How We Share Your Information

We do not sell your personal information. We may share it with:

- **Service providers:** [LIST: e.g., Stripe (payments), Google Analytics (analytics), Mailchimp (email), AWS (hosting)] — bound by data processing agreements
- **Business transfers:** In connection with a merger, acquisition, or sale of assets
- **Legal compliance:** When required by law, subpoena, or to protect our rights
- **With your consent:** For any other purpose with your explicit consent

## 5. Cookies and Tracking Technologies

We use cookies and similar technologies to operate and improve the Service.

| Cookie Type | Purpose | Examples |
|------------|---------|---------|
| Strictly Necessary | Service operation, security | Session cookies |
| Analytics | Usage measurement and improvement | Google Analytics |
| [Marketing] | [Personalized advertising] | [Meta Pixel, Google Ads] |

You can control cookies through your browser settings. Disabling certain cookies may affect Service functionality.

[IF GDPR / EU USERS:]
We obtain your consent before setting non-essential cookies. You may withdraw consent at any time via our cookie preference center.

## 6. Data Retention

We retain your personal data for as long as your account is active or as needed to provide the Service. [PLACEHOLDER: specific retention periods, e.g., "Account data is deleted within 30 days of account closure. Usage logs are retained for 12 months."]

## 7. Your Rights

**All users:**
- Access, correct, or delete your personal data by contacting [CONTACT EMAIL]
- Opt out of marketing communications via unsubscribe link or account settings

**GDPR (EU/EEA Users):**
You have the right to: access, rectification, erasure ("right to be forgotten"), restriction of processing, data portability, and to object to processing. To exercise these rights, contact [CONTACT EMAIL]. You may also lodge a complaint with your local supervisory authority.

**CCPA (California Users):**
You have the right to know what personal information we collect, request deletion, and opt out of sale of personal information. We do not sell personal information. To submit a request, contact [CONTACT EMAIL] or visit [PRIVACY REQUEST URL].

## 8. Children's Privacy

[IF COPPA APPLIES:]
The Service is not directed to children under 13. We do not knowingly collect personal information from children under 13. If we learn we have collected such information, we will delete it promptly. Contact [CONTACT EMAIL] if you believe a child has provided us personal data.

[IF NO COPPA:]
The Service is not intended for users under [13/18]. We do not knowingly collect data from minors.

## 9. Security

We implement industry-standard security measures including [encryption in transit (TLS), encryption at rest, access controls, and regular security reviews]. No method of transmission over the Internet is 100% secure; we cannot guarantee absolute security.

## 10. International Transfers

[IF APPLICABLE:]
Your information may be transferred to and processed in countries other than your own, including the United States. We ensure appropriate safeguards are in place (e.g., Standard Contractual Clauses for EU transfers).

## 11. Changes to This Policy

We may update this Privacy Policy periodically. We will notify you of material changes via [email / in-app notice]. Continued use of the Service after the effective date constitutes acceptance.

## 12. Contact Us

For privacy questions or to exercise your rights:
[COMPANY NAME]
Email: [PRIVACY CONTACT EMAIL]
[ADDRESS if applicable]
[DATA PROTECTION OFFICER (if GDPR required): [DPO NAME/EMAIL]]

---

*This document was generated for informational purposes only and does not constitute legal advice. Review with qualified legal counsel before publishing.*
```

---

### Anti-Spam Policy Template

```markdown
# Anti-Spam Policy
**[APP NAME]**
Effective Date: [DATE]
Last Updated: [DATE]

---

## 1. Our Commitment

[COMPANY NAME] is committed to responsible communication practices. We do not send unsolicited commercial messages and comply with applicable anti-spam laws including CAN-SPAM (US)[, CASL (Canada)][, and GDPR email provisions (EU)].

## 2. Consent-Based Communication Only

We only send marketing and promotional communications to users who have explicitly opted in. We maintain records of consent including the date, method, and scope of each user's opt-in.

[IF DOUBLE OPT-IN:]
We use a double opt-in process: after subscribing, users receive a confirmation email and must click a link to confirm their subscription before receiving any marketing communications.

## 3. What We Send

We may send the following types of messages:

| Type | Description | Requires Opt-In |
|------|-------------|-----------------|
| Transactional | Account notices, receipts, security alerts | No (necessary for service) |
| [Newsletter / Marketing] | [Product updates, promotions, tips] | Yes |
| [SMS Alerts] | [Order updates, security codes] | Yes |

## 4. How to Opt Out

**Email:** Every marketing email contains an unsubscribe link. Click it to be removed from that list within [10 business days / immediately]. You may also unsubscribe via [account settings URL] or by emailing [CONTACT EMAIL].

[IF SMS:]
**SMS:** Reply STOP to any marketing text message. You will receive one confirmation message and no further marketing texts. Reply HELP for help or contact [CONTACT EMAIL].

Opting out of marketing does not affect transactional communications required to operate your account.

## 5. CAN-SPAM Compliance (US)

In compliance with the CAN-SPAM Act, all commercial emails we send:
- Identify [COMPANY NAME] as the sender
- Include our physical mailing address: [ADDRESS]
- Contain a clear and conspicuous unsubscribe mechanism
- Are honored within 10 business days of opt-out request
- Do not use deceptive subject lines or sender information
- Are clearly identified as advertisements where applicable

## 6. CASL Compliance (Canada)

[IF CANADA USERS:]
For users in Canada, we comply with the Canadian Anti-Spam Legislation (CASL):
- We obtain express consent before sending commercial electronic messages (CEMs)
- Consent records are maintained with date, method, and scope
- Every CEM includes our identification and contact information
- Unsubscribe requests are honored within 10 business days

## 7. GDPR Email Compliance (EU/EEA)

[IF EU USERS:]
For users in the EU/EEA:
- Marketing emails are sent only with explicit prior consent (opt-in)
- Consent is freely given, specific, informed, and unambiguous
- You may withdraw consent at any time without detriment
- Consent records are maintained as required under GDPR Article 7

## 8. Third-Party Email Providers

We use [EMAIL PROVIDER, e.g., Mailchimp / SendGrid / PLACEHOLDER] to send emails. This provider acts as a data processor under our data processing agreement and may not use your email address for their own marketing.

## 9. Prohibited Use by Users

[IF PLATFORM WITH MESSAGING FEATURES:]
Users of [APP NAME] may not use our platform or APIs to:
- Send unsolicited bulk messages (spam)
- Harvest email addresses or contact information without consent
- Forge headers or misrepresent message origin
- Send messages that violate any applicable anti-spam law

Violations may result in immediate account suspension and reporting to relevant authorities.

## 10. Reporting Spam

If you believe you received unsolicited email from us, please report it to [CONTACT EMAIL]. We investigate all reports and take corrective action where warranted.

## 11. Contact

[COMPANY NAME]
Email: [CONTACT EMAIL]
[PHYSICAL ADDRESS]

---

*This document was generated for informational purposes only and does not constitute legal advice. Review with qualified legal counsel before publishing.*
```

---

### Cookie Policy Template

```markdown
# Cookie Policy
**[APP NAME]**
Effective Date: [DATE]
Last Updated: [DATE]

---

## 1. What Are Cookies

Cookies are small text files stored on your device when you visit a website. We also use similar technologies such as pixels, web beacons, and local storage.

## 2. Cookies We Use

| Cookie Name / Category | Type | Purpose | Duration |
|------------------------|------|---------|----------|
| Session cookie | Strictly Necessary | Keeps you logged in | Session |
| csrf_token | Strictly Necessary | Prevents cross-site request forgery | Session |
| [_ga, _gid] | Analytics | [Google Analytics — measures usage] | [2 years / 24 hours] |
| [_fbp] | Marketing | [Meta Pixel — ad measurement] | [90 days] |
| [PLACEHOLDER] | [TYPE] | [PURPOSE] | [DURATION] |

**Strictly Necessary:** Required to operate the Service. Cannot be disabled.
**Analytics:** Help us understand how users interact with the Service. Disabled unless you consent.
**Marketing:** Used to deliver relevant advertising. Disabled unless you consent.

## 3. Managing Cookies

[IF CONSENT BANNER:]
When you first visit [APP NAME], we ask for your consent to non-essential cookies. You can change your preferences at any time via the cookie settings [link / footer button].

You can also control cookies through your browser:
- Chrome: Settings → Privacy and Security → Cookies
- Firefox: Preferences → Privacy & Security
- Safari: Preferences → Privacy
- Edge: Settings → Privacy, Search and Services

Note: Disabling strictly necessary cookies will prevent the Service from functioning correctly.

## 4. Third-Party Cookies

Some cookies are set by third-party services we use. These parties have their own privacy policies:
- [Google Analytics: policies.google.com/privacy]
- [PLACEHOLDER: other third parties]

## 5. Contact

[COMPANY NAME]
Email: [CONTACT EMAIL]

---

*This document was generated for informational purposes only and does not constitute legal advice. Review with qualified legal counsel before publishing.*
```

---

### Acceptable Use Policy Template

```markdown
# Acceptable Use Policy
**[APP NAME]**
Effective Date: [DATE]
Last Updated: [DATE]

---

## 1. Overview

This Acceptable Use Policy ("AUP") governs your use of [APP NAME] and any related services, APIs, or platforms operated by [COMPANY NAME]. By using the Service, you agree to this AUP.

## 2. Prohibited Activities

You may not use the Service to:

**Legal violations:**
- Violate any applicable local, national, or international law or regulation
- Infringe any patent, trademark, trade secret, copyright, or other intellectual property rights
- Engage in fraud, phishing, identity theft, or financial crimes

**Harmful content:**
- Post, upload, or distribute content that is defamatory, obscene, hateful, or incites violence
- Harass, threaten, bully, or intimidate other users
- Distribute child sexual abuse material (CSAM) — zero tolerance, reported to NCMEC

**Technical abuse:**
- Attempt to gain unauthorized access to any part of the Service or its systems
- Conduct denial-of-service (DoS) attacks or load testing without written permission
- Introduce malware, viruses, trojans, or other malicious code
- Scrape, crawl, or harvest data from the Service using automated means without authorization
- Circumvent rate limits, access controls, or security measures

[IF API PROVIDED:]
**API abuse:**
- Use the API in a manner that exceeds rate limits or degrades service for others
- Resell or redistribute API access without written authorization
- Use the API to build a competing product that replicates core Service functionality

[IF UGC PLATFORM:]
**Content standards:**
- Do not post spam, duplicate content, or off-topic material
- Do not impersonate any person, company, or entity
- Do not post others' private information without consent (doxxing)
- [PLACEHOLDER: platform-specific content rules]

## 3. Enforcement

Violations of this AUP may result in, at our sole discretion:
- Warning and required remediation
- Temporary suspension of access
- Permanent termination of account
- Legal action and/or reporting to law enforcement

We reserve the right to investigate suspected violations and cooperate with law enforcement.

## 4. Reporting Violations

Report AUP violations to [CONTACT EMAIL]. We investigate all credible reports.

## 5. Changes

We may update this AUP at any time. Continued use of the Service constitutes acceptance.

## 6. Contact

[COMPANY NAME]
Email: [CONTACT EMAIL]

---

*This document was generated for informational purposes only and does not constitute legal advice. Review with qualified legal counsel before publishing.*
```

---

### DMCA / Copyright Policy Template

```markdown
# DMCA Copyright Policy
**[APP NAME]**
Effective Date: [DATE]
Last Updated: [DATE]

---

## 1. Respect for Intellectual Property

[COMPANY NAME] respects intellectual property rights and expects users to do the same. We respond to notices of alleged copyright infringement in compliance with the Digital Millennium Copyright Act (DMCA), 17 U.S.C. § 512.

## 2. Designated Copyright Agent

To submit a copyright infringement notice, contact our designated agent:

**[DMCA AGENT NAME]**
[COMPANY NAME]
[ADDRESS]
Email: [DMCA CONTACT EMAIL]
[Phone: OPTIONAL]

## 3. Submitting a Takedown Notice

To be valid, your DMCA notice must include:
1. Identification of the copyrighted work you claim has been infringed
2. Identification of the infringing material and its location on our Service (URL or description)
3. Your contact information (name, address, phone, email)
4. A statement that you have a good faith belief the use is not authorized by the copyright owner, its agent, or law
5. A statement that the information in your notice is accurate and, under penalty of perjury, that you are the copyright owner or authorized to act on their behalf
6. Your physical or electronic signature

Send notices to: [DMCA CONTACT EMAIL]

We will respond to valid notices within [10–14] business days.

## 4. Counter-Notice

If you believe your content was removed in error, you may submit a counter-notice to [DMCA CONTACT EMAIL] including:
1. Identification of the removed material and its former location
2. A statement under penalty of perjury that you have a good faith belief the material was removed by mistake or misidentification
3. Your name, address, phone number, and consent to jurisdiction of federal court in your district
4. Your physical or electronic signature

Upon receipt of a valid counter-notice, we will restore the material within 10–14 business days unless the complainant files a court action.

## 5. Repeat Infringer Policy

We maintain a policy of terminating accounts of users who are repeat infringers of intellectual property rights, in appropriate circumstances.

## 6. Contact

[COMPANY NAME]
Email: [DMCA CONTACT EMAIL]

---

*This document was generated for informational purposes only and does not constitute legal advice. Review with qualified legal counsel before publishing.*
```

---

## Generation Guardrails

- **Jurisdiction matters:** Always ask if EU/California/Canada users exist — these require specific legal sections
- **Not legal advice:** Every generated document ends with the standard disclaimer
- **No hallucination:** Do not invent company details; use `[PLACEHOLDER]` instead
- **COPPA warning:** If user says app may have under-13 users, flag that COPPA requires verifiable parental consent — recommend legal counsel
- **App Store note:** For mobile apps, note that Apple/Google have their own review guidelines; privacy policy URL must be submitted to both stores
- **GDPR DPA:** If B2B SaaS processing EU customer data, mention a Data Processing Agreement (DPA) is likely required in addition to a privacy policy
- **Paid apps:** If app takes payments, note PCI DSS compliance is handled by payment processor (Stripe/etc.) but terms must reference it
- **Already executed docs:** If user has existing policies, offer to review (switch to Review Mode) rather than regenerate from scratch

