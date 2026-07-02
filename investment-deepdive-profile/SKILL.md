---
name: investment-deepdive-profile
description: Generates a Chinese-language institutional VC/PE deep-dive due-diligence profile (深度分析画像/尽调画像) for a startup, from an uploaded BP/pitch deck plus supplementary materials (team bios, papers, decks, data rooms), enriched with public web research on the team, technology, competitors, and market. Produces a formatted Word (.docx) memo with cover page, table of contents, company overview, core team, technology/product deep-dive, competitive landscape with moat scoring, and an investment judgment section (positive signals, risk signals, open due-diligence questions, overall rating). Use this whenever the user uploads or references a BP/pitch deck/招股书/项目材料 and asks for a 投资画像, 尽调报告, 深度分析, deep dive, investment memo, company profile, or due diligence write-up — especially for technology-driven startups (AI, biotech, deep tech, hard science) with papers/patents/benchmarks to dig into. Also trigger when the user says things like "帮我看看这个项目", "生成一份内部画像", "整理一下尽调材料" alongside an uploaded document.
---

# Investment Deep-Dive Profile Generator

Produces institutional-grade Chinese investment due-diligence memos in the style of a corporate VC/CVC internal research team: dense, table-heavy, opinionated, and explicit about what is verified vs. still unknown. This is not a generic "summarize the BP" task — the output must read like an analyst who did real diligence work, cross-checked claims against public sources, and formed a point of view.

## When to use this

Trigger whenever the user gives you a BP, pitch deck, team bios, technical papers, or a data-room bundle and wants an analytical writeup for internal investment decision-making — not just a summary of the deck's own claims. Also trigger for follow-up requests like "帮我更新一下这份画像" (refresh with new materials) or "把这份 BP 也整理成同样格式" (same format for a different company).

## Before you start: gather three things

1. **Source materials** — the BP/deck itself, plus whatever supplementary files the user has (team resumes, published papers, product one-pagers, prior notes). Read every file provided; don't skip any on the assumption the BP alone is enough — supplementary materials are usually where the sharpest details live (a specific benchmark number, a named investor, a specific team credential).
2. **Branding/config** — ask the user (unless already told in this conversation):
   - Institution/team label for the cover page and footer (e.g. "工银投资 · 国创引导基金专项柔性团队"). If the user has no institutional brand, use a neutral placeholder like "内部尽调材料" — don't invent a firm name.
   - Confidentiality footer text (default: "本材料仅供内部参阅，不构成投资建议" / "本材料仅供内部参阅，不得外传" — adjust wording to match what the user gives you).
   - Report date (default: today).
   - Any known deal context worth cross-referencing (e.g. "we already have portfolio companies in adjacent space X" — this feeds the "与投资组合的关联" note at the end).
3. **Depth signal** — is this a technology-driven company (papers, patents, benchmarks, a research team) or a business/product-driven one (GTM, unit economics, market share)? This changes how you build Section 3 — see "Adapting Section 3" below. Don't ask this as a separate question if it's obvious from the materials; just decide.

## Workflow

### 1. Extract everything from the source materials
Read the BP and every supplementary file completely (use the `pdf`, `docx`, `pptx`, or `xlsx` skills as appropriate for each file type — don't skim). Pull out, verbatim where possible:
- Company legal name, one-line positioning, stage, funding history, team size
- Named team members with titles and claimed credentials (schools, prior companies, publications, patents)
- Every quantitative claim: benchmark numbers, revenue, user counts, model parameters, experiment results — note the exact number and its source (which document, which page)
- Every named technology/product/paper/patent
- Any named customers, partners, investors, or competitors mentioned in the materials

Keep a running list of claims that are **asserted by the company but not yet independently verified** — this list becomes both your web-research task list and, for anything you can't verify, a line item in the risk-signals table later. This distinction (verified vs. company-asserted) is the single most important thing separating this report from a repackaged BP summary.

### 2. Web research to verify and extend
For each named team member, search for their public profile (prior company track record, publications, LinkedIn/公开报道) — confirm or flag discrepancies with what the BP claims. For each named technology/paper, verify it exists and check citation/reception if discoverable (conference acceptance, GitHub stars, benchmark leaderboards). For the market, search for the 2-4 most relevant competitors (direct competitors doing the same thing, and adjacent players solving the same problem differently) and pull recent news, funding rounds, or product launches for each. For any named investor or customer, verify via public sources where possible.

Do not present unverified BP claims as fact. When you cannot verify something (private company data, unannounced funding, informal introductions), say so explicitly rather than silently repeating the company's framing — the analyst voice in this report is skeptical by default and only asserts what it can support.

### 3. Write the report following the fixed structure

Follow `references/report_template.md` for the exact section structure, subsection breakdown, and the tone/content guidance for each part — read it now if you haven't already. The structure (cover → TOC → overview → team → tech/product → competition → judgment → appendix) is fixed; don't reorder or drop sections even for a thin BP — if a section has little material, say so plainly (e.g. "团队规模未披露") rather than omitting the section.

**Adapting Section 3 (技术/产品底盘):** for research/deep-tech companies, mirror the reference sample's "layered tech stack + paper matrix" format — organize by technical layer, cite specific papers/benchmarks per layer, close with a synthesized "投资含义" (what the stack implies for investability: closed-loop completeness, methodological consistency, TAM expansion, core uncertainty). For product/business-driven companies, replace this with a **产品与商业模式** section covering: product architecture, core product metrics/traction, business model and unit economics, and a synthesized "商业化含义" paragraph — same analytical rigor and same "positive data + open uncertainty" pattern, just applied to product/GTM evidence instead of papers.

Every major subsection should end with a short synthesis paragraph in the analyst's own voice (labeled things like "团队判断", "竞争格局核心判断", "综合判断") — this is where you state an actual opinion, not just recite facts. Always pair a strength with a named risk or open question; a synthesis paragraph that is pure cheerleading or pure caution is a sign you haven't actually weighed the evidence.

### 4. Render to Word

Build a `content.json` following the schema documented at the top of `scripts/build_report_docx.py`, then run:
```
python3 scripts/build_report_docx.py content.json output.docx
```
This produces the cover page, native Word TOC field, styled section headings, bordered/shaded tables, callout boxes for synthesis paragraphs, and a running footer with page numbers and the confidentiality text on every page. Open the script's docstring for the exact JSON shape before writing `content.json` — matching the schema exactly (block `type` values, table `headers`/`rows` shape) is what makes the formatting come out right; don't improvise new block types.

After generating, skim the .docx (via the `docx` skill's read helpers, or by re-extracting text) to sanity-check that every section landed and no table is empty — then tell the user where the file is and flag anything you couldn't verify or that needs a real due-diligence conversation to resolve (this maps to the "核心待验证问题" section — surface it in your chat reply too, not just buried in the document).

## Tone and craft notes

- Chinese, formal-but-direct analyst register — short declarative sentences, numbers over adjectives ("50x酶活性提升（湿实验验证）" beats "显著提升").
- Tables over prose wherever there's more than 2-3 parallel items (team members, competitors, papers, signals).
- Every strong claim gets a number or a named source when the materials support it; when they don't, say "未披露" or "待验证" rather than inventing precision.
- The investment judgment section must contain real tension — list risks that could genuinely kill the deal, not token caveats. A report with six positive signals and zero real risks will not be trusted by the reader and defeats the purpose of the memo.
- Close with concrete next-step questions for the next management conversation, not generic advice.
