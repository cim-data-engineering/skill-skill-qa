---
name: skill-qa
description: >
  Reviews and QA's Claude skills against official best practices. Produces a
  structured report with pass/fail/warning grades across frontmatter, description
  quality, conciseness, progressive disclosure, structure, content, and workflow
  patterns. Use when the user asks to "QA this skill", "review my skill",
  "check my skill", "audit this skill", "validate my SKILL.md", "skill quality
  check", or any request to evaluate or improve a skill against best practices.
---

# Skill QA

Systematically reviews a Claude skill against official best practice principles and produces a graded QA report with actionable recommendations.

Before starting a review, read [references/validation-checklist.md](references/validation-checklist.md) for the specific pass/fail criteria for each check. Consult [references/anti-patterns.md](references/anti-patterns.md) to identify common mistakes.

---

## Workflow Overview

```
1. LOCATE     → Find the skill's SKILL.md and supporting files
2. INVENTORY  → Catalog all files in the skill directory
3. REVIEW     → Evaluate 7 QA dimensions
4. SCORE      → Grade each dimension (PASS / WARN / FAIL)
5. REPORT     → Output structured QA report
6. RECOMMEND  → Provide prioritized, actionable improvements
```

### Progress Checklist

Use this to track review progress:

- [ ] LOCATE — SKILL.md found and read
- [ ] INVENTORY — all skill files cataloged
- [ ] REVIEW — all 7 dimensions evaluated
- [ ] SCORE — grades assigned
- [ ] REPORT — QA report delivered
- [ ] RECOMMEND — improvement priorities listed

---

## Step 1: LOCATE

Identify which skill to review:

- If the user provides a **path**, read the SKILL.md at that location
- If the user says **"this skill"** or **"my skill"**, look for SKILL.md in the current working directory
- If unclear, **ask the user** which skill to review

Read the SKILL.md file completely. If no SKILL.md is found, stop and report:

> **FAIL: No SKILL.md found at {path}.** A valid skill requires a file named exactly `SKILL.md`. Create one with YAML frontmatter containing `name` and `description` fields.

---

## Step 2: INVENTORY

List all files in the skill directory and categorize them:

- **SKILL.md** — note the line count
- **references/** — list all files and their line counts
- **scripts/** — list all files
- **assets/** — list all files
- **Other files** — flag anything unexpected (especially README.md inside the skill folder)

Record the total file count and SKILL.md line count for the report header.

---

## Step 3: REVIEW

Evaluate the skill across 7 dimensions. For each dimension, reference the specific check IDs from `references/validation-checklist.md`.

### Dimension 1: Frontmatter Validation

Check the YAML frontmatter (checks F1–F9):

- `---` delimiters present on opening and closing lines
- `name`: exists, ≤ 64 chars, kebab-case only (`^[a-z0-9][a-z0-9-]*$`), no "claude"/"anthropic", no XML tags
- `description`: exists, non-empty, ≤ 1024 chars, no XML tags

### Dimension 2: Description Quality

Evaluate the description field (checks D1–D7):

- Written in **third person** (not "I can..." or "You can...")
- Clearly states **what** the skill does
- Includes **when** to trigger with specific phrases
- Specific enough — not vague ("helps with projects")
- Covers casual user phrasings, not just formal commands
- Scoped appropriately — not so broad it over-triggers

### Dimension 3: Conciseness

Assess token efficiency (checks C1–C4):

- SKILL.md body under **500 lines** (WARN at 400+, FAIL at 500+)
- Does not explain things Claude already knows (basic concepts, standard formats)
- No redundant repetition across sections
- Each section justifies its token cost

### Dimension 4: Progressive Disclosure

Check the three-level loading system (checks P1–P6):

- Frontmatter is self-contained for relevance decisions
- SKILL.md body serves as a high-level guide, not an exhaustive reference
- Reference files are **linked with context** ("Read X when Y")
- References are **one level deep** — no chained references (A → B → C)
- Reference files over 100 lines include a **table of contents**
- Large data sets or schemas live in reference files, not inline

### Dimension 5: Structure & Naming

Validate conventions (checks S1–S6):

- Folder name is kebab-case
- File is named exactly `SKILL.md`
- No `README.md` inside the skill folder
- All file path references use **forward slashes**
- Subdirectories follow convention (`references/`, `scripts/`, `assets/`)
- Filenames are descriptive (not `doc1.md` or `utils.py`)

### Dimension 6: Content Quality

Evaluate the instruction content (checks Q1–Q6):

- No **time-sensitive information** that will become stale
- **Consistent terminology** — same concept uses the same term throughout
- **Concrete examples** present (code snippets, input/output pairs, templates)
- **Error handling** documented — what to do when things go wrong
- Instructions are **specific and actionable** — not "validate properly"
- Bundled resources **referenced clearly** with file paths and purpose

### Dimension 7: Workflow & Patterns

Assess workflow design (checks W1–W6):

- Complex tasks have **numbered sequential steps**
- Multi-step workflows include a **progress checklist**
- Critical operations have **validation/feedback loops** (run → check → fix → repeat)
- **Patterns match task type** — templates for output format, examples for demonstrations, conditionals for decision points
- **Degrees of freedom calibrated** — fragile tasks get specific guardrails, flexible tasks get high-level guidance
- **Defaults with escape hatches** — one recommended approach, alternatives for edge cases

### Conditional: Script & Code Checks

If the skill has a `scripts/` directory, also evaluate checks X1–X7 from the validation checklist. Key areas: error handling in scripts, documented dependencies, no magic numbers, fully qualified MCP tool names.

---

## Step 4: SCORE

### Per-Dimension Grading

| Grade | Meaning | Criteria |
|-------|---------|----------|
| **PASS** | Meets best practices | No issues found in this dimension |
| **WARN** | Minor issues | Works but could be improved; non-blocking |
| **FAIL** | Significant issues | Violates a core principle; should be fixed before use |

A dimension's grade is determined by its worst individual check result.

### Overall Grade

| Grade | Criteria |
|-------|----------|
| **A** (Excellent) | All PASS, at most 1 WARN |
| **B** (Good) | 0 FAIL, 2+ WARN |
| **C** (Needs Work) | Exactly 1 FAIL |
| **D** (Significant Issues) | 2+ FAIL |
| **F** (Not Ready) | SKILL.md missing or fundamentally broken |

---

## Step 5: REPORT

Output the QA report using this exact template:

```markdown
# Skill QA Report: {skill-name}

**Overall Grade: {A/B/C/D/F}**
**Reviewed:** {date}
**Files:** {count} files in skill directory
**SKILL.md length:** {N} lines

## Dimension Scores

| # | Dimension              | Grade | Summary |
|---|------------------------|-------|---------|
| 1 | Frontmatter Validation | {grade} | {one-line finding} |
| 2 | Description Quality    | {grade} | {one-line finding} |
| 3 | Conciseness            | {grade} | {one-line finding} |
| 4 | Progressive Disclosure | {grade} | {one-line finding} |
| 5 | Structure & Naming     | {grade} | {one-line finding} |
| 6 | Content Quality        | {grade} | {one-line finding} |
| 7 | Workflow & Patterns    | {grade} | {one-line finding} |

## Findings

### FAIL items (fix these first)
{numbered list with check IDs, specific line references, and fix instructions}

### WARN items (recommended improvements)
{numbered list with check IDs and improvement suggestions}

### PASS items
{brief acknowledgment of what the skill does well}

## Top 3 Recommendations
1. {highest-priority actionable improvement}
2. {second priority}
3. {third priority}
```

---

## Step 6: RECOMMEND

- Prioritize **FAIL items first**, then high-impact WARN items
- Each recommendation must be **actionable**: state what to change, where, and why
- Include specific text suggestions where possible (e.g., "Change description to: ...")
- **Offer to help** implement fixes if the user wants
- If the skill scored **A**, congratulate and suggest only minor polish

---

## Edge Cases

- **No references/ directory**: Not an error — but note if SKILL.md is long and could benefit from splitting content
- **Scripts present**: Add the Script & Code Checks dimension to the review
- **Monorepo with multiple skills**: Ask the user which skill to review, or offer to review each sequentially
- **Reviewing skill-qa itself**: Acknowledge the circularity, but still apply the same checks honestly
- **Incomplete skill (work in progress)**: Note which dimensions cannot be fully evaluated and grade only what exists
