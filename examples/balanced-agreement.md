# Balanced Agreement Example - Low Risk

This example shows what a well-balanced contract looks like and how to report when terms are acceptable.

## Prompt

```
I'm the customer. Review this SaaS agreement.
```

## Expected Output for Well-Balanced Agreement

```xml
<contract_review>
  <summary>
    <document_type>SaaS Subscription Agreement</document_type>
    <user_position>Customer</user_position>
    <parties>
      <vendor>Acme Software Inc.</vendor>
      <customer>Your Company</customer>
    </parties>
    <effective_date>January 1, 2025</effective_date>
    <risk_level>Low</risk_level>
    <executive_summary>Well-balanced commercial agreement with mutual protections. Terms are within market standards for enterprise SaaS. Minor negotiation points exist but no critical issues requiring attention before signing.</executive_summary>
  </summary>

  <key_terms>
    <term name="Initial Term">
      <value>12 months</value>
      <location>Section 8.1</location>
    </term>
    <term name="Auto-Renewal">
      <value>12-month periods with 90-day notice to cancel</value>
      <location>Section 8.2</location>
    </term>
    <term name="Liability Cap">
      <value>12 months' fees paid</value>
      <location>Section 10.2</location>
    </term>
    <term name="SLA">
      <value>99.9% uptime with 10% service credits</value>
      <location>Exhibit B</location>
    </term>
    <term name="Governing Law">
      <value>Delaware</value>
      <location>Section 12.1</location>
    </term>
  </key_terms>

  <risk_analysis>
    <risk category="Price Adjustments" severity="Low" priority="Minor">
      <clause>"Vendor may adjust pricing upon renewal with 60 days notice, not to exceed 5% annually"</clause>
      <issue>Price increases are capped but could compound over multi-year relationship</issue>
      <recommendation>
        <position>Request CPI-linked cap instead of flat 5%</position>
        <redline>
          <original>not to exceed 5% annually</original>
          <proposed>not to exceed the greater of (i) 3% or (ii) the annual change in the Consumer Price Index</proposed>
        </redline>
        <fallback>Acceptable as-is if other terms are favorable</fallback>
      </recommendation>
    </risk>
  </risk_analysis>

  <reviewed_acceptable>
    <category name="Limitation of Liability" note="12-month cap is market standard; mutual application"/>
    <category name="Indemnification" note="Mutual indemnification with reasonable scope"/>
    <category name="Data Ownership" note="Customer owns all customer data; clear export rights"/>
    <category name="Termination" note="Mutual termination for convenience with 90 days notice"/>
    <category name="Auto-Renewal" note="90-day notice period is adequate"/>
    <category name="SLA" note="99.9% with meaningful credits; termination right for chronic failure"/>
    <category name="Governing Law" note="Delaware is neutral for commercial agreements"/>
    <category name="IP Rights" note="Customer retains all IP in customer data and configurations"/>
    <category name="Confidentiality" note="Mutual 3-year term; reasonable exceptions"/>
    <category name="Assignment" note="Neither party may assign without consent; change of control exception"/>
  </reviewed_acceptable>

  <internal_consistency>
    <!-- No issues found -->
  </internal_consistency>

  <disclaimer>This review is for informational purposes only and does not constitute legal advice. Material terms should be reviewed by qualified legal counsel.</disclaimer>
</contract_review>
```

## What Makes This Low Risk

### Liability & Risk
- **12-month liability cap** - Market standard for enterprise SaaS
- **Mutual indemnification** - Both parties protect each other
- **Carve-outs make sense** - IP infringement, gross negligence uncapped (standard)

### Termination & Renewal
- **Mutual termination for convenience** - Either party can exit
- **90-day notice** - Adequate time to plan transition
- **Reasonable cure period** - 30 days to fix breaches

### Data & IP
- **Customer owns data** - Clear statement
- **Export rights** - Can get data out in standard format
- **No vendor lock-in** - Transition assistance included

### Service Levels
- **99.9% uptime commitment** - Industry standard
- **Meaningful credits** - 10% per percentage point below SLA
- **Termination right** - Can exit if chronic SLA failures

### Financial
- **Price increase cap** - 5% annual maximum
- **No hidden fees** - All fees disclosed upfront
- **Pro-rata refunds** - For prepaid unused services

## Key Lesson

Not every contract needs extensive redlines. When reviewing a balanced agreement:

1. **Acknowledge what's acceptable** - Use `<reviewed_acceptable>` section
2. **Don't over-flag** - Minor issues as "Low" severity, not everything is "High"
3. **Provide context** - Explain why terms are acceptable
4. **Focus negotiation energy** - Save pushback for truly problematic terms
5. **Recognize market standards** - Don't flag standard terms as risks

## Signs of a Balanced Agreement

- Mutual obligations where appropriate (indemnification, confidentiality, termination)
- Market-standard liability caps (12 months for SaaS)
- Reasonable notice periods (60-90 days)
- Clear data ownership and export rights
- Meaningful SLA with actual remedies
- Price increase caps
- Neither party has dramatically more power
