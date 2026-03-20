# PRD Reviewer Skills

Two Claude-compatible skills for PRD (Product Requirements Document) quality assessment and improvement. Extracted from the [PRD Reviewer](https://prd-reviewer.pages.dev) production system.

## Skills

### 📊 PRDAnalysis
Comprehensive 11-dimension PRD quality scoring framework with weighted subcriteria.

**Dimensions:** Problem Validation (12%) · User Understanding (10%) · Solution & Scope (12%) · Requirements & AC (12%) · BMC Fit (10%) · Technical Feasibility (8%) · Edge Cases (8%) · Metrics (10%) · GTM Readiness (8%) · Document Structure (5%) · Compliance (5%)

**Output:** Structured JSON with per-dimension scores, subcriteria breakdown, evidence-based issues/praise, and a weighted total score with verdict.

### ✍️ PRDRewrite
Lossless PRD rewriting with UC 3.0 (Jacobson & Cockburn 2024) structure enhancement.

**Features:**
- **Lossless editing** — output is equal length or longer than input
- **UC 3.0 enrichment** — adds 9-section Use Case structure if missing
- **Section-level splitting** — handles large PRDs by rewriting sections in parallel
- **Metrics enrichment** — adds Indicator/Success Metrics frameworks

## Setup

### Claude Code
```bash
# Clone into your Claude skills directory
git clone https://github.com/productaivn-wq/prd-reviewer-skills.git
cp -r prd-reviewer-skills/PRDAnalysis ~/.claude/skills/
cp -r prd-reviewer-skills/PRDRewrite ~/.claude/skills/
```

### Antigravity
```bash
git clone https://github.com/productaivn-wq/prd-reviewer-skills.git
cp -r prd-reviewer-skills/PRDAnalysis ~/.gemini/antigravity/skills/
cp -r prd-reviewer-skills/PRDRewrite ~/.gemini/antigravity/skills/
```

## Usage

Once installed, just ask Claude:
- *"Analyze this PRD"* → triggers PRDAnalysis
- *"Rewrite this PRD"* → triggers PRDRewrite
- *"Score this PRD"* → triggers PRDAnalysis
- *"Improve this PRD"* → triggers PRDRewrite

## Language

All analysis output and rewritten PRDs are in **Vietnamese** by default (matching the production system). The scoring framework and rubrics are language-agnostic.

## License

MIT
