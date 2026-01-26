# M&A Agreement Review Example

## Prompt

```
Review this acquisition agreement. I'm the seller. Focus on:
1. Purchase price mechanics (earnouts, escrow, holdbacks)
2. Rep & warranty scope and survival periods
3. Indemnification basket and cap structure
4. Non-compete and employee retention terms
5. Closing conditions and MAC clause
6. Post-closing covenants
```

## M&A-Specific Risk Categories

### Purchase Price Structure
- **Cash at close**: Most certain; optimize for this
- **Earnouts**: Heavily discount value (uncertain, requires continued involvement)
- **Escrow/holdback**: Typically 10-20% for 12-24 months
- **Seller financing**: Interest rate, security, subordination

### Representations & Warranties
- **Scope**: What seller is representing about the business
- **Knowledge qualifiers**: "To Seller's knowledge" vs. absolute
- **Materiality scrapes**: Affect indemnification calculations
- **Survival**: How long reps survive post-close (typically 12-24 months)

### Indemnification
- **Basket/deductible**: Threshold before buyer can claim (typically 0.5-1% of deal value)
- **Cap**: Maximum indemnification exposure (typically 10-20% of purchase price)
- **Carve-outs**: Fraud, fundamental reps usually have higher/no cap
- **Sandbagging**: Can buyer claim for issues they knew about?

### Restrictive Covenants
- **Non-compete**: Duration (2-5 years), geography, scope
- **Non-solicit**: Employees and customers
- **Reasonableness**: May not be enforceable if too broad

## Expected Output

```xml
<contract_review>
  <summary>
    <document_type>Asset Purchase Agreement</document_type>
    <parties>
      <seller>Target Co LLC</seller>
      <buyer>Acquirer Inc.</buyer>
    </parties>
    <effective_date>Upon signing, closing [DATE]</effective_date>
    <risk_level>High</risk_level>
    <executive_summary>$15M acquisition with 30% earnout, 18-month escrow, broad reps with 3-year survival, and 5-year non-compete. Significant seller exposure post-closing.</executive_summary>
  </summary>

  <key_terms>
    <term name="Base Purchase Price">
      <value>$10,500,000 cash at closing</value>
      <location>Section 2.1(a)</location>
    </term>
    <term name="Earnout">
      <value>Up to $4,500,000 based on Year 1-2 revenue</value>
      <location>Section 2.1(b), Exhibit C</location>
    </term>
    <term name="Escrow Amount">
      <value>$1,575,000 (15% of base) for 18 months</value>
      <location>Section 2.1(c)</location>
    </term>
    <term name="Indemnification Cap">
      <value>$3,000,000 (20% of total consideration)</value>
      <location>Section 10.4</location>
    </term>
    <term name="Non-Compete Term">
      <value>5 years, nationwide</value>
      <location>Section 7.8</location>
    </term>
  </key_terms>

  <risk_analysis>
    <risk category="Earnout Mechanics" severity="High">
      <clause>"Earnout shall be calculated based on Revenue as determined by Buyer in its sole discretion using Buyer's standard accounting practices"</clause>
      <issue>No objective measurement standard; buyer controls calculation; no audit rights</issue>
      <recommendation>Define Revenue precisely (GAAP, consistent with historical), add seller audit rights, require operation of business consistent with past practice, add acceleration on change of control</recommendation>
    </risk>

    <risk category="Warranty Duration" severity="High">
      <clause>"All representations and warranties shall survive for 36 months following Closing"</clause>
      <issue>3-year survival is longer than market (typically 12-18 months for general reps)</issue>
      <recommendation>Negotiate to 18 months for general reps; accept longer survival only for fundamental reps (title, authority, capitalization) and tax</recommendation>
    </risk>

    <risk category="Non-Compete" severity="High">
      <clause>"Seller shall not, for 5 years, engage in any business competitive with the Business anywhere in the United States"</clause>
      <issue>5-year nationwide non-compete may not be enforceable and is longer than typical (2-3 years)</issue>
      <recommendation>Reduce to 3 years; limit geography to markets where business actually operates; clearly define "competitive"</recommendation>
    </risk>

    <risk category="Sandbagging" severity="Medium">
      <clause>"Buyer's right to indemnification shall not be affected by any investigation or knowledge"</clause>
      <issue>Pro-sandbagging clause allows buyer to claim for issues they knew about pre-close</issue>
      <recommendation>Add anti-sandbagging: "Buyer may not seek indemnification for matters actually known to Buyer prior to Closing"</recommendation>
    </risk>

    <risk category="Basket Structure" severity="Medium">
      <clause>"First dollar basket of $150,000"</clause>
      <issue>First dollar (not true deductible) means once threshold is met, buyer recovers from dollar one</issue>
      <recommendation>Convert to true deductible (tipping basket) so seller only pays amounts exceeding threshold</recommendation>
    </risk>
  </risk_analysis>

  <missing_provisions>
    <provision name="Earnout Protections">
      <importance>Without these, buyer can manipulate to minimize earnout</importance>
      <suggested_language>"Buyer shall operate the Business consistent with past practice, maintain dedicated accounting, and not take actions primarily intended to reduce Earnout payments. Seller shall have quarterly reporting and annual audit rights."</suggested_language>
    </provision>
    <provision name="R&W Insurance Acknowledgment">
      <importance>If buyer has R&W insurance, seller exposure should be reduced</importance>
      <suggested_language>"Buyer acknowledges it has obtained representation and warranty insurance and shall first seek recovery thereunder before asserting claims against Seller."</suggested_language>
    </provision>
  </missing_provisions>
</contract_review>
```

## Seller Protection Checklist

1. **Maximize cash at close** - Earnouts, escrows, holdbacks all reduce certain value
2. **Earnout protections** - Operating covenants, audit rights, acceleration triggers
3. **Short rep survival** - 12-18 months general, 3-6 years fundamental only
4. **True deductible basket** - Not first-dollar
5. **Cap with teeth** - 10-15% excluding fraud/fundamental
6. **Anti-sandbagging** - Buyer can't claim for known issues
7. **Reasonable non-compete** - 2-3 years, limited geography, clear scope
8. **Escrow release** - Partial at 12 months, full at 18
9. **D&O tail insurance** - Buyer to purchase or fund
10. **Expense cap** - Limit seller transaction expenses that reduce proceeds
