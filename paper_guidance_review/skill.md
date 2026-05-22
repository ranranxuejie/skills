---
name: paper-writing-review
description: Academic paper writing guidance and review checking. Helps users write paper sections (Abstract, Introduction, Methods, Results, Discussion, Conclusion) following top-tier journal standards, or review/audit existing paper drafts against quality checklists. Supports all research types: Clinical/Public Health, Experimental Basic Science, CS/AI, Social Sciences, Engineering.
---

# Paper Writing & Review Skill

This skill provides two modes for academic paper assistance. Load the appropriate guidance/review files dynamically based on the user's request.

---

## Mode Selection

Determine the user's intent and enter the corresponding mode:

### Mode 1: WRITE (Paper Writing Guidance)
**Trigger**: User wants to write, draft, or compose a paper section. Keywords: "write", "draft", "compose", "帮我写", "撰写", "起草".

### Mode 2: REVIEW (Paper Review/Check)
**Trigger**: User wants to review, check, audit, or evaluate an existing paper section. Keywords: "review", "check", "audit", "审稿", "检查", "评审".

### Mode 3: FULL PAPER (Write entire paper)
**Trigger**: User wants to write a complete paper from scratch or wants an overall plan. Keywords: "full paper", "整篇论文", "写一篇论文".

---

## File Loading Protocol

All files are located relative to this skill's directory: `skills/paper_guidance_review/`.

### Section Index

| Section | Guidance File | Review File |
|---|---|---|
| Overall (global rules) | `guidance/overall_guidance.md` | `review/overall_review.md` |
| Abstract | `guidance/abstract_guidance.md` | `review/abstract_review.md` |
| Introduction | `guidance/introduction_guidance.md` | `review/introduction_review.md` |
| Methods | `guidance/methods_guidance.md` | `review/methods_review.md` |
| Results | `guidance/results_guidance.md` | `review/results_review.md` |
| Discussion | `guidance/discussion_guidance.md` | `review/discussion_review.md` |
| Conclusion | `guidance/conclusion_guidance.md` | `review/conclusion_review.md` |

### Loading Rules

1. **Always load `overall_guidance.md` or `overall_review.md` first** as the global foundation.
2. **Then load the section-specific file(s)** matching the user's request.
3. **For FULL PAPER mode**: Load all guidance files in order: overall -> abstract -> introduction -> methods -> results -> discussion -> conclusion.
4. **If user specifies multiple sections**: Load each section's guidance/review file.
5. **If section is ambiguous**: Ask the user which section(s) they want to focus on.

### How to Load Files

Use the `Read` tool with absolute path constructed from the skill directory. Example:
```
Read file: skills/paper_guidance_review/guidance/abstract_guidance.md
```

---

## Mode 1: WRITE — Writing Guidance Workflow

### Step 1: Gather Requirements
Before writing, collect from the user:
- **Research type**: Clinical/Public Health, Experimental Basic Science, CS/AI, Social Sciences, or Engineering
- **Target journal tier**: Comprehensive top (Nature/Science) or specialized top (Lancet/NeurIPS/etc.)
- **Section to write**: Abstract, Introduction, Methods, Results, Discussion, Conclusion, or Full Paper
- **Key content** (if available): Research question, methods used, key findings, raw data/draft

### Step 2: Load Guidance Files
Load `overall_guidance.md` + the relevant section guidance file(s). Read them fully.

### Step 3: Apply Guidance Structure
Follow the loaded guidance strictly:
- Use the prescribed **logical framework** (e.g., Inverted Pyramid for abstract, CARS for introduction)
- Follow **proportion guidelines** (e.g., Background 10-15%, Results 40-50% in abstract)
- Apply **differentiated patterns** for the specified research type
- Use **sentence templates** from Section III of the guidance file
- Respect **taboos** (no citations in abstract, no results in methods, etc.)

### Step 4: Generate Draft
Produce the draft in the specified section, ensuring:
- Active voice dominant, appropriate tense usage
- Hedged language (no absolute claims)
- Quantitative evidence where data is available
- If user provided raw data, integrate it; if not, use placeholders like `[INSERT: specific metric here]`

### Step 5: Self-Check Against Review
After generating the draft, load the corresponding `*_review.md` file and run a quick self-check:
- Mark any items that need user attention with `[CHECK]`
- List items from the quality checklist that are satisfied vs. need verification

---

## Mode 2: REVIEW — Review/Check Workflow

### Step 1: Receive Paper Text
Ask the user to provide the paper section(s) to review, either:
- Paste the text directly
- Provide a file path to read

### Step 2: Identify Research Type
Determine or ask for:
- Research type (Clinical, Basic Science, CS/AI, Social Sciences, Engineering)
- Target journal type (comprehensive vs. specialized)

### Step 3: Load Review Files
Load `overall_review.md` + the relevant section review file(s). Read them fully.

### Step 4: Systematic Review
For each loaded review file, check against its quality checklist dimensions:

**Abstract Review** — Check: Completeness, Rigor, Logic, Norms. Flag pitfalls: overly long background, vague results, method-result confusion.

**Introduction Review** — Check: Completeness, Logic, Rigor, Norms. Flag pitfalls: data dump, missing gap, over/under-citing. Check for red lines: plagiarism, self-promotion, informal tone.

**Methods Review** — Check: Reproducibility, Compliance, Logic, Rigor, Norms. Flag pitfalls: insufficient detail, information imbalance, logical breaks, methods-results mismatch. Check red lines: ethical gaps, unclear data provenance.

**Results Review** — Check: Objectivity, Matchability, Logic, Rigor, Norms. Flag pitfalls: results-discussion mixing, text-figure repetition, selective reporting. Check red lines: data fabrication, p-hacking, HARKing.

**Discussion Review** — Check: Boundary Clarity, Logical Completeness, Interpretation Depth, Rigor & Honesty, Normative Compliance. Flag pitfalls: results repetition, over-interpretation, literature dump, hidden limitations. Check red lines: overstating novelty, ignoring contradictory evidence.

**Conclusion Review** — Check: Conciseness, Alignment, No New Data, No New Arguments, Tone, Linkage, Impact, Future, Consistency, Clarity. Flag traps: Repetition, New Information, Disconnect, Overstatement, Vagueness, Linkage.

### Step 5: Produce Review Report
Output a structured review with:
1. **Summary**: Overall quality rating and main issues (2-3 sentences)
2. **Section-by-section findings**: For each checked dimension, list PASS/WARN/FAIL with specific evidence from the text
3. **Pitfall alerts**: Any high-frequency errors detected, with quotes from the text
4. **Red line violations**: Any absolute red lines crossed (critical)
5. **Improvement suggestions**: Specific, actionable fixes with reference to the guidance files

---

## Mode 3: FULL PAPER — Complete Paper Workflow

### Step 1: Gather Full Requirements
Collect:
- Title (if available)
- Research type and target journal
- Research question / hypothesis
- Methods and materials
- Key findings and data
- Any existing draft sections

### Step 2: Create Writing Plan
Based on `overall_guidance.md`, outline the paper structure with section word counts and key points.

### Step 3: Write Section by Section
Write in this order:
1. **Methods** (most concrete, easiest to start)
2. **Results** (data-driven)
3. **Introduction** (sets up the gap that methods/results address)
4. **Discussion** (interprets results in context)
5. **Conclusion** (synthesizes everything)
6. **Abstract** (written last, summarizes the complete paper)

For each section, load its guidance file and apply the framework.

### Step 4: Cross-Section Consistency Check
After all sections are drafted:
- Verify Introduction-Conclusion alignment (gap echoed, hypothesis confirmed)
- Verify Methods-Results consistency (all methods have corresponding results)
- Verify Results-Discussion boundary (no interpretation in results, no data repetition in discussion)
- Check overall logical flow and terminology consistency

### Step 5: Self-Review
Load all review files and run a comprehensive check across all sections. Produce a final quality report.

---

## Important Principles

1. **Source of Truth**: All numbers, benchmarks, dataset details, baselines, and figure/table references must come from existing approved material. Never invent data.
2. **Flexible Reading**: Always Read files fresh — never assume cached content. The guidance files may be updated.
3. **Research-Type Aware**: Always ask for or infer the research type, as guidance varies significantly across types.
4. **Template Reuse**: Leverage the sentence templates in Section III of each guidance file. Adapt them to the user's specific content.
5. **Hedging**: Always use hedged language (suggest, indicate, may, potentially). Never use absolute claims (prove, perfect, definitively).
