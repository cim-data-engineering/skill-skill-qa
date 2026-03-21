# Skill QA

A Claude skill that reviews other skills against official best practice principles. Produces a graded QA report with specific findings and actionable recommendations.

## Installation

### Claude Desktop / Claude.ai

1. Download this repo (Code > Download ZIP, or clone it)
2. Zip the skill folder if not already zipped
3. Open Claude.ai > Settings > Capabilities > Skills
4. Click "Upload skill" and select the zip file
5. Toggle the skill on

### Claude Code

Place or symlink the skill into your `.claude/skills/` directory:

```bash
ln -s /path/to/skill-skill-qa ~/.claude/skills/skill-qa
```

## Usage

Ask Claude to review any skill:

- "QA this skill" (when in a skill's working directory)
- "Review my skill at /path/to/my-skill"
- "Check my skill" / "Audit this skill"
- "Validate my SKILL.md"

## What It Checks

| # | Dimension | What It Evaluates |
|---|-----------|-------------------|
| 1 | Frontmatter Validation | YAML syntax, name/description field rules |
| 2 | Description Quality | Third person, trigger phrases, specificity |
| 3 | Conciseness | Line count, token efficiency, redundancy |
| 4 | Progressive Disclosure | Three-level loading, reference linking, TOC |
| 5 | Structure & Naming | Kebab-case, file conventions, path format |
| 6 | Content Quality | Examples, error handling, terminology, actionability |
| 7 | Workflow & Patterns | Steps, checklists, validation loops, degrees of freedom |

Scripts are additionally checked if a `scripts/` directory exists.

## Output

A structured QA report containing:

- **Overall letter grade** (A–F)
- **Per-dimension grades** (PASS / WARN / FAIL) with one-line summaries
- **Detailed findings** grouped by severity
- **Top 3 prioritized recommendations** with specific fix instructions

## Grading Scale

| Grade | Meaning |
|-------|---------|
| A | Excellent — all pass, at most 1 warning |
| B | Good — no failures, 2+ warnings |
| C | Needs work — 1 failure |
| D | Significant issues — 2+ failures |
| F | Not ready — SKILL.md missing or broken |
