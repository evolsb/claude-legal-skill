# SaaS Agreement Review Example

## Prompt

```
Review this SaaS agreement. I'm the customer. Focus on:
1. Data ownership and portability
2. Uptime/SLA commitments and remedies
3. Limitation of liability (especially caps)
4. Auto-renewal and termination rights
5. Price increase provisions
6. Security and compliance obligations
```

## Key SaaS Contract Provisions to Check

### Data Rights
- **Ownership**: Customer should own all customer data
- **Portability**: Right to export data in standard format
- **Deletion**: Vendor must delete data on termination
- **Subprocessors**: Notice of third-party data access

### Service Levels
- **Uptime commitment**: 99.9% is standard, 99.99% is premium
- **Measurement**: How is uptime calculated? Excludes maintenance windows?
- **Remedies**: Service credits are standard; termination rights for chronic failures
- **Credit caps**: Many limit credits to 1 month's fees

### Liability
- **Cap**: Typically 12 months' fees; watch for lower caps
- **Carve-outs**: Data breach, IP infringement, gross negligence usually uncapped
- **Consequential damages**: Usually waived but verify mutual

### Term & Renewal
- **Auto-renewal**: Standard but check notice period (30 days is tight)
- **Price increases**: Should be capped (CPI or 5% max)
- **Termination for convenience**: Customer should have this right
- **Effect of termination**: Data export period, transition assistance

## Expected Output

```xml
<contract_review>
  <summary>
    <document_type>SaaS Subscription Agreement</document_type>
    <parties>
      <vendor>Acme Software Inc.</vendor>
      <customer>Your Company</customer>
    </parties>
    <effective_date>Upon signature</effective_date>
    <risk_level>High</risk_level>
    <executive_summary>One-sided vendor agreement with 3-month fee liability cap, no uptime SLA, and broad vendor termination rights. Significant negotiation needed.</executive_summary>
  </summary>

  <key_terms>
    <term name="Initial Term">
      <value>36 months</value>
      <location>Section 8.1</location>
    </term>
    <term name="Annual Fee">
      <value>$120,000/year</value>
      <location>Exhibit A</location>
    </term>
    <term name="Liability Cap">
      <value>3 months' fees ($30,000)</value>
      <location>Section 9.2</location>
    </term>
  </key_terms>

  <risk_analysis>
    <risk category="Limitation of Liability" severity="High">
      <clause>"In no event shall Vendor's liability exceed the fees paid in the three (3) months preceding the claim"</clause>
      <issue>3-month cap is unusually low; standard is 12 months. For a $120K annual contract, liability capped at $30K.</issue>
      <recommendation>Negotiate to 12 months' fees ($120K) at minimum. Consider higher cap or unlimited for data breach/security incidents.</recommendation>
    </risk>

    <risk category="Service Level Agreement" severity="High">
      <clause>No SLA provisions found</clause>
      <issue>No uptime commitment or service credits for outages</issue>
      <recommendation>Add SLA exhibit with 99.9% uptime, defined measurement period, and service credits (10% for <99.9%, 25% for <99%, termination right for <95% two consecutive months)</recommendation>
    </risk>

    <risk category="Price Restrictions" severity="Medium">
      <clause>"Vendor may adjust pricing upon any renewal with 30 days notice"</clause>
      <issue>No cap on price increases; 30 days is insufficient notice</issue>
      <recommendation>Cap increases at CPI+3% or 5% max. Require 90 days notice.</recommendation>
    </risk>
  </risk_analysis>

  <missing_provisions>
    <provision name="Data Export Rights">
      <importance>Without this, you may lose access to your data on termination</importance>
      <suggested_language>"Upon termination, Vendor shall make Customer Data available for export in a standard machine-readable format for 90 days at no additional charge."</suggested_language>
    </provision>
    <provision name="Security Standards">
      <importance>No defined security obligations or compliance certifications</importance>
      <suggested_language>"Vendor shall maintain SOC 2 Type II certification and notify Customer of any security incidents within 24 hours of discovery."</suggested_language>
    </provision>
  </missing_provisions>
</contract_review>
```

## Vendor-Favorable Terms to Watch For

1. **Liability cap tied to recent fees** - On annual contracts, "3 months' fees" is very low
2. **Broad indemnification from customer** - Should be mutual
3. **Vendor can terminate for convenience** - Customer should have this, not vendor
4. **No uptime commitments** - Even if not SLA, should have some commitment
5. **Unlimited price increases** - Always cap renewals
6. **Assignment to acquirer** - Watch for "including by merger" language
7. **Audit rights without limits** - Should have notice and frequency limits
