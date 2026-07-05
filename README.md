# Claude-Skills

Personal collection of [Claude Code](https://claude.com/claude-code) skills.

## Skills

### investment-deepdive-profile

Generates a Chinese-language institutional VC/PE deep-dive due-diligence profile (深度分析画像) for a startup, from an uploaded BP/pitch deck plus supplementary materials, enriched with public web research. Produces a formatted Word (.docx) memo with cover page, table of contents, company overview, core team, technology/product deep-dive, competitive landscape, and an investment judgment section (positive signals, risk signals, open due-diligence questions, overall rating).

See [`investment-deepdive-profile/SKILL.md`](investment-deepdive-profile/SKILL.md) for full usage.

### aic-equity-dd-report

Generates a Chinese-language formal institutional 股权投资尽职调查报告（尽调报告）in the bank/AIC 股权投资试点业务 format — anchored to a concrete deal (fund vehicle, valuation, legal/buyback terms) with a 声明与保证 signature page, financial statement trend analysis, credit/compliance checks, industry and peer-comparison research, IRR-based return scenarios, and a numbered risk/应对措施 section. Enforces a strict no-fabrication rule: every figure must trace to provided materials or a verifiable public source, otherwise marked 未披露/待核实. Supports chapter-by-chapter drafting with a final integration pass for large data rooms.

See [`aic-equity-dd-report/SKILL.md`](aic-equity-dd-report/SKILL.md) for full usage.

## Usage

Copy a skill folder into `~/.claude/skills/` (or your project's `.claude/skills/`) to make it available in Claude Code.
