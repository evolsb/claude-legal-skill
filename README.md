# Claude Legal Contract Review Skill

A Claude Code skill for reviewing legal contracts. Built on validated patterns from:
- **Anthropic's Legal Team** - XML-structured output patterns
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
```

## Features

### Risk Identification
Scans for 41 CUAD risk categories including:
- Termination for convenience
- Change of control triggers
- Unlimited liability exposure
- One-sided indemnification
- Broad IP assignments
- Non-compete overreach

### Structured Output
Returns XML-formatted analysis for easy parsing:
- Executive summary with risk rating
- Key terms extraction
- Clause-by-clause risk analysis
- Missing provisions check
- Prioritized redline suggestions

### M&A Support
Special handling for acquisition-related agreements:
- Earnout mechanics
- Rep & warranty analysis
- Escrow/holdback provisions
- Non-compete evaluation

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
- **Jurisdiction**: Analysis assumes US law unless specified; provisions vary by jurisdiction

## Accuracy

Based on ContractEval benchmarks (2025):
- Claude Sonnet achieves F1 ~0.62 on clause extraction
- Performs at "junior legal assistant" level
- Best for first-pass review and issue flagging, not final legal judgment

## Contributing

PRs welcome! Areas for improvement:
- Additional jurisdiction support (UK, EU, etc.)
- More specialized contract types (real estate, healthcare, etc.)
- Integration with additional legal AI APIs

## License

MIT License - see [LICENSE](LICENSE)

## Credits

- [CUAD Dataset](https://github.com/TheAtticusProject/cuad) - Atticus Project
- [LegalBench](https://hazyresearch.stanford.edu/legalbench/) - Stanford HAI
- [Anthropic Legal Summarization Guide](https://docs.anthropic.com)
