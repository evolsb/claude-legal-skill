# Claude Legal Contract Review Skill

A Claude Code skill for reviewing legal contracts. Identifies risks, extracts key terms, and suggests specific redline language.

Built on validated patterns from:
- **CUAD Dataset** (NeurIPS 2021) - 41 clause risk categories from 510 commercial contracts
- **ContractEval Benchmark** - Peer-reviewed LLM evaluation for legal tasks
- **LegalBench** (Stanford) - 162 legal reasoning tasks

## Installation

### Option 1: Clone to Claude Skills Directory

```bash
git clone https://github.com/yourusername/claude-legal-skill.git ~/.claude/skills/contract-review
```

### Option 2: Symlink from Your Clone

```bash
git clone https://github.com/yourusername/claude-legal-skill.git ~/Developer/claude-legal-skill
ln -s ~/Developer/claude-legal-skill ~/.claude/skills/contract-review
```

## Usage

In Claude Code, the skill activates when you:
- Ask to "review a contract" or "analyze this agreement"
- Upload or reference a PDF/DOCX legal document
- Ask about specific clauses, risks, or terms

### Example Commands

```
Review this NDA for red flags
Analyze the indemnification terms in this MSA
What are the termination provisions in this agreement?
Extract key terms from this SaaS contract
I'm the customer - review this vendor agreement
```

## Features

### Position-Aware Review
Tell the skill which party you are (customer, vendor, buyer, seller) and it adjusts what it flags as risky. A vendor-favorable term is a risk for the customer, not the vendor.

### Risk Identification
Scans for all 41 CUAD risk categories including:
- Termination for convenience
- Change of control triggers
- Unlimited liability exposure
- One-sided indemnification
- Broad IP assignments
- Non-compete overreach

### Structured XML Output
Returns parseable analysis:
- Executive summary with risk rating
- Key terms extraction with locations
- Risk analysis with severity and priority
- Specific redline suggestions (original → proposed)
- Missing provisions with suggested language
- Internal consistency checks

### Jurisdiction Awareness
Flags when governing law affects enforceability:
- Non-competes void in CA/ND/OK
- Delaware vs NY vs CA choice of law implications
- Arbitration clause enforceability

### M&A Support
Special handling for acquisition agreements:
- Earnout mechanics
- Rep & warranty analysis
- Escrow/holdback provisions
- Working capital adjustments
- MAC clause review

## Output Example

```xml
<contract_review>
  <summary>
    <document_type>SaaS Subscription Agreement</document_type>
    <user_position>Customer</user_position>
    <risk_level>Medium</risk_level>
    <executive_summary>Standard vendor agreement with some one-sided terms...</executive_summary>
  </summary>

  <risk_analysis>
    <risk category="Limitation of Liability" severity="High" priority="Critical">
      <clause>"Liability shall not exceed fees paid in the preceding 3 months"</clause>
      <issue>3-month cap is below market standard (typically 12 months)</issue>
      <recommendation>
        <position>Request 12-month cap</position>
        <redline>
          <original>three (3) months</original>
          <proposed>twelve (12) months</proposed>
        </redline>
        <fallback>Accept 6 months as compromise</fallback>
      </recommendation>
    </risk>
  </risk_analysis>

  <reviewed_acceptable>
    <category name="Governing Law" note="Delaware - standard for commercial agreements"/>
    <category name="IP Ownership" note="Customer retains all customer data"/>
  </reviewed_acceptable>
</contract_review>
```

## Optional: Zuva Integration

For automated clause extraction using 1,354+ pre-trained legal fields:

1. Create free account at [dashboard.zuva.ai](https://dashboard.zuva.ai)
2. Generate API token
3. Configure:
```bash
pip install git+https://github.com/zuvaai/zdai-python.git
python -m zdai --set-token YOUR_TOKEN
python -m zdai --set-url https://us.app.zuva.ai/api/v2
python -m zdai --test connection
```

## Limitations

- **Not legal advice**: This tool is for informational purposes. Always have material terms reviewed by qualified counsel.
- **Context window**: Very long contracts may need to be reviewed in sections
- **Jurisdiction**: Analysis defaults to US law; provisions vary by jurisdiction

## Accuracy

Based on ContractEval benchmarks:
- Claude achieves F1 ~0.62 on clause extraction
- Best for first-pass review and issue flagging
- Not a replacement for attorney review on material deals

## Contributing

PRs welcome! Areas for improvement:
- Additional jurisdiction support (UK, EU, etc.)
- More specialized contract types (real estate, healthcare, etc.)
- Industry-specific checklists

## License

MIT License - see [LICENSE](LICENSE)

## Credits

- [CUAD Dataset](https://github.com/TheAtticusProject/cuad) - Atticus Project
- [LegalBench](https://hazyresearch.stanford.edu/legalbench/) - Stanford HAI
- [ContractEval](https://arxiv.org/abs/2303.07389) - Contract understanding benchmarks
