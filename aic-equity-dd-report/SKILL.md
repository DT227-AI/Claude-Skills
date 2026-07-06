---
name: aic-equity-dd-report
description: Generates a Chinese-language formal institutional 股权投资尽职调查报告（尽调报告）in the fixed bank/AIC (金融资产投资公司) 股权投资试点业务 structure used for internal投决/审批 — the "关于为XX公司办理不超过X亿元股权投资试点业务的尽调报告" format, NOT the early-stage-VC BP deep-dive format (use investment-deepdive-profile for that). Covers: 声明与保证/尽调人签署页, 项目方案（交易要素、基金结构、双GP、回购条款）, 标的企业基本情况与财务分析（资产负债/利润/现金流三表趋势分析）, 资信情况（行内评级、征信、诉讼、对外担保）, 行业技术分析与竞争优势, 同业可比分析, 投资收益测算（IRR情景分析）, 上市退出可行性、风险及防控措施（编号风险+应对措施配对格式）, 调查结论性意见（明确投资建议）. Enforces a strict no-fabrication rule — every figure/claim must trace to provided materials (审计报告/征信报告/工商信息/访谈纪要/基金及交易文件) or a verifiable public source (企业信用信息公示系统/裁判文书网/上市公司公告/官方行业报告), otherwise it must be marked 未披露/待核实. Supports drafting part-by-part with a final integration pass when the data room is large. Trigger when the user asks for a 尽调报告/尽职调查报告 in this bank-pilot-equity style, or mentions 基金方案、交易要素、回购条款、行内评级、资信情况、IRR测算、上市退出可行性 alongside project materials.
---

# Institutional Equity Investment Due-Diligence Report (股权投资试点业务尽调报告)

Produces the formal due-diligence report a bank-affiliated AIC (金融资产投资公司) or similar institutional investor submits internally to support an equity investment decision — the kind that carries a 尽职调查人/复核人/总经理签字页 and reads like a compliance-grade record of what was actually checked, not a pitch. This is a different artifact from a VC-style BP deep-dive: it is anchored to a specific deal (amount, valuation, fund vehicle, legal terms) and to verifiable facts (audited financials, credit bureau data, court records), and its credibility depends entirely on not inventing anything.

## When to use this vs. `investment-deepdive-profile`

- Use **this skill** when the ask is a formal 尽调报告/尽职调查报告 tied to a concrete deal: an investment amount, a fund/SPV vehicle, deal terms (估值、回购、优先权), and internal sign-off. Signals: user mentions 基金方案、交易要素、双GP、回购条款、行内评级、资信情况、投决会、IRR测算, or provides bank/AIC-style materials (审计报告、征信报告、工商信息、基金合同).
- Use `investment-deepdive-profile` when the ask is an early-stage analytical memo built mostly from a BP/pitch deck, without a concrete deal structure yet.
- If unclear which one fits, ask.

## The one rule that overrides everything else: no fabrication

Every sentence with a number, date, name, or percentage in the final report must be traceable to one of:
1. **Provided materials** — BP/商业计划书, 审计报告/未经审计报表, 征信报告, 工商登记信息, 基金合同/合伙协议/增资协议, 访谈纪要, 尽调资料包 (data room) files.
2. **Verifiable public information** — 国家企业信用信息公示系统, 裁判文书网/全国法院被执行人查询系统, 上市公司公告/定期报告 (for comparable companies), official industry-research publications, company/government official sites. Use WebSearch/WebFetch and note what you actually found — don't present a plausible-sounding but unverified figure as fact.
3. **Explicitly attributed management/expert statements** — anything from an interview or the company's own claim that you cannot independently verify must be phrased as "据公司反馈"/"根据管理层介绍"/"访谈XX专家了解到", never stated as an established fact.

When a data point is genuinely unavailable, write **"未披露"**, **"资料未提供，待补充"**, or **"需进一步核实"** — never invent a plausible-sounding number, name, date, or percentage to fill a gap. This applies especially to: financial statement figures, shareholding percentages, financing round amounts/valuations, litigation/credit records, and headcount. Forward-looking numbers (盈利预测, IRR测算) must always be phrased as assumptions ("假设……", "按XX折扣测算") with the assumption stated, never as bare projected fact.

Before writing, build a **source map**: for each of the 7 parts below, list which provided documents or verified public sources will supply its content, and which sub-sections have no source yet. Anything with no source gets marked 未披露 in the draft rather than skipped silently — silently dropping a required sub-section reads as sloppy, not as appropriately cautious.

## Depth calibration: match the reference sample's length, not a summary

The reference sample this skill is built from runs to roughly **30,000+ Chinese characters of running prose** (plus tables) across the 7 parts — this is a full institutional research document, not an executive summary. A common failure mode is producing a thin, bullet-heavy draft that hits every required heading but at a fraction of that depth. `references/report_template.md` has a **per-section character-count calibration table** derived from the actual reference sample — check your draft against it before rendering, and if a section is running short, expand it the right way:

- **Expand by adding granularity, not filler.** More length must come from covering more real things — every balance-sheet line item gets its own variance sentence, every team member gets their own bio paragraph, every named competitor gets a full profile, every risk gets its own numbered sub-point when the source material supports it. Never pad by restating the same fact in more words or adding generic filler sentences ("公司发展前景广阔" and similar) — that fails the no-fabrication spirit just as much as inventing a number does.
- **The two heaviest parts are Part 2/三(财务情况) and Part 2/一(基本情况 + 公司治理).** In the reference sample these alone account for over half the document, because every account in the balance sheet/income statement/cash flow gets its own driver sentence, and every core team member gets a full-paragraph bio with named prior employers, credentials, and specific achievements — not a one-line title.
- **Part 3(行业分析) and Part 4(同业可比分析) are the other long sections**, driven by covering every sub-topic of the technology/industry background (background → principles → classification → trends → applications → challenges, each as its own developed paragraph) and giving each named competitor a genuine multi-sentence profile (founding, location, tech route, product status, financials if public, funding history) rather than a one-line mention.
- **Some sections are naturally short and shouldn't be padded to match** — 资信情况 (mostly confirmation of query results) and 结论性意见 (a dense, decisive closing judgment) are supposed to be compact. Depth calibration means matching the *right* sections' depth, not applying a uniform word count everywhere.
- If the provided materials genuinely don't support the reference sample's depth for a section (e.g. a much smaller/simpler deal with less team/financial history), that's fine — say so rather than inventing detail to hit a target. But check this is actually true before assuming it; most thin drafts are thin because the model summarized available material too aggressively, not because the material was sparse.

## Before you start: gather materials and confirm scope

Ask for (or inventory from what's already provided) — don't stall on all of these being present, but note gaps explicitly:
- **Deal terms**: target company, investment vehicle (fund/SPV name and structure), investment amount, pre-money valuation, use of proceeds, planned exit route.
- **Company materials**: BP/商业计划书, 工商登记信息/股权结构, 公司治理文件 (章程/董事会决议), 历次融资协议摘要 (轮次/金额/估值/主要条款).
- **Financials**: 近三年审计报告 + 最新未经审计报表 (资产负债表/利润表/现金流量表), 审计机构与审计意见.
- **Credit/compliance**: 行内评级、授信记录, 人行征信报告, 裁判文书/被执行人查询结果, 对外担保情况.
- **Fund/legal**: 基金合同或合伙协议 (GP/LP结构、决策机制、管理费、收益分配、存续期), 本轮融资的保障条款与回购条款.
- **Diligence record**: 访谈纪要 (管理层/投资人/客户/行业专家), 现场调研记录.
- **Institution identity**: 提交单位/团队名称, 尽职调查人/复核人/负责人姓名及联系方式 (or ask the user to supply — don't invent names for the signature page), 提交日期.

If a whole category is missing (e.g. no credit bureau report provided), say so to the user before drafting Part 2/四(资信情况) and mark that subsection 未披露 rather than skip it or fabricate it.

## Workflow

### 1. Build the source map
Read every provided file completely (use the `docx`/`pdf`/`xlsx`/`pptx` skills as appropriate — don't skim). For each of the 7 parts in `references/report_template.md`, note what source material covers it. Flag gaps now, not after drafting.

### 2. Draft part-by-part (chapter-and-integrate for large data rooms)
Follow the fixed structure in `references/report_template.md` — section order and coverage are fixed; don't reorder or silently drop a part even when thin on material. For a normal-sized deal, draft straight through. **When the data room is large enough that drafting the full report in one pass risks losing fidelity (long financial history, many competitors, long interview transcripts), draft one part at a time as an independent chapter**, in this order:
1. 第一部分 项目方案 (项目背景、尽调实施情况、交易要素、基金方案、融资条款)
2. 第二部分 标的企业情况 (基本情况、经营情况、财务情况、资信情况)
3. 第三部分 行业分析与公司优势
4. 第四部分 同业可比分析
5. 第五部分 投资收益情况分析
6. 第六部分 上市退出可行性、风险分析及防控措施
7. 第七部分 调查结论性意见

After each chapter, save it as a JSON fragment matching the `blocks` schema in `scripts/build_report_docx.py` to the scratchpad directory (e.g. `part1.json` … `part7.json`) before moving to the next — this both protects against context loss and lets you integrate deterministically at the end instead of re-deriving earlier chapters from memory. Part 7 (结论性意见) must be drafted last and must synthesize tension actually present in Parts 1–6 (real risks vs. real positives), not restate the company's own pitch.

### 3. Web research for Parts 3–5
Part 3 (行业分析) and Part 4 (同业可比分析) lean on public information — search for the technology/industry background and each named competitor's public facts (founding date, funding history, product specs, financials if listed) and cite what you actually found. Part 5's comparable-company multiples (for IRR/valuation benchmarking) should use real listed peers with real reported figures, not invented ones — if no clean public comp exists, say so and use the closest available proxy with the caveat stated.

### 4. Integrate and self-check
Merge the chapter fragments into one `content.json` per the schema documented at the top of `scripts/build_report_docx.py`. Before rendering, run two passes over the merged draft:
1. **No-fabrication pass**: for every number/name/date, confirm it's in your source map from Step 1 — if you can't place its source, either verify it or replace it with 未披露/待核实.
2. **Depth pass**: check each part's prose length against the calibration table in `references/report_template.md`. Any part running noticeably under its reference range gets expanded by adding the missing granularity (more line items, more team bios, more competitor detail, more risk sub-points) pulled from the source materials you already have or from further web research — not by padding sentences.

### 5. Render to Word
```
python3 scripts/build_report_docx.py content.json output.docx
```
This renders the declaration/signature page, the fixed 第X部分 section numbering with 一/二/三 and (一)(二)(三) sub-levels, bordered financial tables, and the risk-item/应对措施 pairing format used throughout Part 6.

After generating, skim the .docx to confirm every part landed, no table is empty, and the risk section actually pairs each numbered risk with its 应对措施. Report back to the user: where the file is, which sections had missing source material (marked 未披露), and what open items still need real diligence input before this could be submitted for approval.

## Tone and format conventions (match the source format exactly)

- Formal third-person institutional register — declarative, no marketing language. Chinese business-report numbering style: 第一部分/第二部分 → 一/二/三 → （一）（二）（三） → 1./2./3.
- Units in 万元 or 亿元 as the source materials use; financial tables include a same-period-last-year (同比变化%) column when multi-year data exists — this is the convention throughout Part 2/三(财务情况).
- Every financial trend claim should be a specific number/percentage tied to a named account/subject (e.g. "货币资金较上年末增加X万元，主要为……"), followed by the driver — never a vague adjective alone.
- Part 6 risk items follow a fixed pattern per item: numbered risk title → concrete description grounded in the source materials → a paragraph starting "应对措施：" — never a risk item without a corresponding mitigation.
- Part 7 (结论性意见) must end with an explicit, specific recommendation: whether to proceed, at what amount/valuation/structure, and under what conditions — mirroring how Part 1's 交易要素 was framed. It is a decision memo, not a summary.
- Anything sourced from a management interview rather than a document must say so explicitly ("据公司反馈"/"根据管理层介绍") — this distinction is load-bearing for a compliance-grade report, not a style nicety.
