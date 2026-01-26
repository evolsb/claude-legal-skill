# NDA Review Example

## Prompt

```
Review this NDA for any red flags. Focus on:
1. Definition of confidential information (is it too broad?)
2. Term and survival periods
3. Non-compete or non-solicitation provisions (often hidden in NDAs)
4. One-sided obligations
5. Governing law and jurisdiction
```

## Expected Output Structure

The skill will return XML-structured analysis:

```xml
<contract_review>
  <summary>
    <document_type>Non-Disclosure Agreement (Mutual/One-Way)</document_type>
    <parties>
      <disclosing_party>...</disclosing_party>
      <receiving_party>...</receiving_party>
    </parties>
    <effective_date>...</effective_date>
    <risk_level>Medium</risk_level>
    <executive_summary>This is a one-way NDA with broad confidentiality definitions and a 5-year survival period. The non-solicitation clause is unusual for an NDA.</executive_summary>
  </summary>

  <key_terms>
    <term name="Confidentiality Definition">
      <value>"All information disclosed..." - Very broad, includes oral</value>
      <location>Section 1.1, Page 1</location>
    </term>
    <term name="Term">
      <value>2 years from Effective Date</value>
      <location>Section 5, Page 3</location>
    </term>
    <term name="Survival">
      <value>5 years post-termination</value>
      <location>Section 5.2, Page 3</location>
    </term>
  </key_terms>

  <risk_analysis>
    <risk category="Confidentiality Duration" severity="Medium">
      <clause>"The obligations of confidentiality shall survive for five (5) years following termination"</clause>
      <issue>5-year survival is longer than standard (typically 2-3 years for general business info)</issue>
      <recommendation>Negotiate to 2-3 years, or add carve-out for trade secrets which can survive longer</recommendation>
    </risk>
    <risk category="Non-Solicitation" severity="High">
      <clause>"Receiving Party shall not solicit any employees of Disclosing Party for 12 months"</clause>
      <issue>Non-solicitation is unusual in an NDA and may not be enforceable depending on jurisdiction</issue>
      <recommendation>Remove or move to a separate agreement if employment discussions are contemplated</recommendation>
    </risk>
  </risk_analysis>

  <missing_provisions>
    <provision name="Residuals Clause">
      <importance>Protects use of general knowledge retained in unaided memory</importance>
      <suggested_language>"Nothing in this Agreement shall restrict the use of Residuals. 'Residuals' means information retained in the unaided memories of Representatives who have had access to Confidential Information."</suggested_language>
    </provision>
  </missing_provisions>
</contract_review>
```

## Common NDA Red Flags

1. **Overly broad definitions** - "All information" without reasonable carve-outs
2. **One-way obligations** - Only one party bound to confidentiality
3. **Long survival periods** - 5+ years for non-trade-secret info
4. **Hidden restrictive covenants** - Non-compete, non-solicit buried in NDA
5. **No residuals clause** - Can't use general knowledge gained
6. **Injunctive relief** - Pre-agreed specific performance without bond
7. **Broad assignability** - Other party can assign without consent
