# Validation Checklist

Use this checklist to evaluate a skill systematically. Each check has an ID for traceability in the QA report.

## Contents

1. [Frontmatter Checks (F1–F9)](#1-frontmatter-checks)
2. [Description Checks (D1–D7)](#2-description-checks)
3. [Conciseness Checks (C1–C4)](#3-conciseness-checks)
4. [Progressive Disclosure Checks (P1–P6)](#4-progressive-disclosure-checks)
5. [Structure & Naming Checks (S1–S6)](#5-structure--naming-checks)
6. [Content Quality Checks (Q1–Q6)](#6-content-quality-checks)
7. [Workflow & Pattern Checks (W1–W6)](#7-workflow--pattern-checks)
8. [Script & Code Checks (X1–X7)](#8-script--code-checks) *(only if scripts/ exists)*
9. [Quick Pre-Flight Checklist](#9-quick-pre-flight-checklist)

---

## 1. Frontmatter Checks

| ID | Check | PASS | FAIL |
|----|-------|------|------|
| F1 | YAML delimiters present | Opens and closes with `---` | Missing or malformed delimiters |
| F2 | `name` field exists | Present and non-empty | Missing or empty |
| F3 | `name` max length | ≤ 64 characters | > 64 characters |
| F4 | `name` format | Lowercase letters, numbers, hyphens only (`^[a-z0-9][a-z0-9-]*$`) | Uppercase, underscores, spaces, or special characters |
| F5 | `name` no reserved words | Does not contain "claude" or "anthropic" | Contains reserved words |
| F6 | `name` no XML tags | No `<` or `>` characters | Contains angle brackets |
| F7 | `description` field exists | Present and non-empty | Missing or empty |
| F8 | `description` max length | ≤ 1024 characters | > 1024 characters |
| F9 | `description` no XML tags | No `<` or `>` characters | Contains angle brackets |

---

## 2. Description Checks

| ID | Check | PASS | FAIL | WARN |
|----|-------|------|------|------|
| D1 | Third person voice | No "I can", "I will", "You can", "You should" | First or second person used throughout | — |
| D2 | Includes WHAT | Clearly states what the skill does | Purpose unclear or missing | — |
| D3 | Includes WHEN (triggers) | Contains specific trigger phrases ("Use when...") | No trigger conditions at all | Trigger phrases present but too few or too formal |
| D4 | Not vague | Specific about domain, actions, or file types | Generic phrasing like "helps with projects", "processes data" | — |
| D5 | Mentions file types | References relevant file types/extensions if applicable | File-oriented skill that omits file types | N/A for non-file skills |
| D6 | Includes casual trigger variations | Covers how users naturally phrase requests | Only formal/technical trigger phrases | — |
| D7 | Avoids over-triggering | Scoped to specific domain; negative triggers if needed | Description so broad it would match unrelated queries | — |

---

## 3. Conciseness Checks

| ID | Check | PASS | FAIL | WARN |
|----|-------|------|------|------|
| C1 | SKILL.md body length | Under 400 lines | Over 500 lines | 400–500 lines |
| C2 | No verbose Claude-obvious content | Only adds context Claude does not already have | Explains basic concepts (what a PDF is, how REST APIs work, etc.) | — |
| C3 | No redundant repetition | Each concept stated once in the appropriate place | Same instructions repeated across multiple sections | — |
| C4 | Token cost justified | Each paragraph/section serves a clear purpose | Filler content, unnecessary preambles, or padding | — |

---

## 4. Progressive Disclosure Checks

| ID | Check | PASS | FAIL | WARN |
|----|-------|------|------|------|
| P1 | Frontmatter is self-contained | Frontmatter provides enough for Claude to decide relevance | Frontmatter references body content to understand purpose | — |
| P2 | SKILL.md is high-level guide | Body provides overview and links to details | Body contains exhaustive detail that should be in references | — |
| P3 | References linked with guidance | Links include when/why to read each file | References exist but are not linked from SKILL.md | References linked but without context on when to read |
| P4 | References one level deep | All reference files link directly from SKILL.md | Chained references (A → B → C) | — |
| P5 | Large files have TOC | Reference files > 100 lines include a table of contents | Large reference files with no TOC | — |
| P6 | No inline exhaustive data | Large data sets, schemas, or catalogs are in reference files | Hundreds of lines of reference data inline in SKILL.md | — |

---

## 5. Structure & Naming Checks

| ID | Check | PASS | FAIL |
|----|-------|------|------|
| S1 | Folder name is kebab-case | Lowercase letters, numbers, hyphens only | Spaces, underscores, capitals, or special characters |
| S2 | SKILL.md exact filename | File is named exactly `SKILL.md` (case-sensitive) | Any variation (skill.md, SKILL.MD, Skill.md) |
| S3 | README.md appropriate use | README.md absent or used only for installation/usage instructions | README.md duplicates SKILL.md content or contains skill logic that belongs in SKILL.md |
| S4 | Forward slashes in paths | All file path references use `/` | Backslashes (`\`) in any path reference |
| S5 | Conventional subdirectories | Uses `references/`, `scripts/`, `assets/` as appropriate | Non-standard directory names (docs/, lib/, helpers/) |
| S6 | Descriptive filenames | File names indicate content (e.g., `api-patterns.md`) | Vague names (doc1.md, file2.py, utils.md) |

---

## 6. Content Quality Checks

| ID | Check | PASS | FAIL | WARN |
|----|-------|------|------|------|
| Q1 | No time-sensitive information | No dates that will become stale; or legacy info in collapsible sections | Hard-coded dates or "as of 202X" that will become wrong | — |
| Q2 | Consistent terminology | Same concept uses the same term throughout | Mixed terms for the same concept (e.g., "endpoint"/"route"/"URL" interchangeably) | — |
| Q3 | Concrete examples present | Includes specific examples, code snippets, or input/output pairs | Purely abstract instructions with no examples | — |
| Q4 | Error handling documented | Common failure modes and recovery steps described | No mention of what to do when things go wrong | — |
| Q5 | Instructions are actionable | Specific commands, steps, or criteria given | Vague directives like "validate properly" or "handle errors" | — |
| Q6 | Resources referenced clearly | Bundled files referenced with explicit path and purpose | References like "see the docs" without specific file paths | — |

---

## 7. Workflow & Pattern Checks

| ID | Check | PASS | FAIL | WARN |
|----|-------|------|------|------|
| W1 | Sequential steps for complex tasks | Multi-step processes have numbered, ordered steps | Complex workflow described as a block of prose | — |
| W2 | Checklists for multi-step workflows | Progress checklist provided for workflows with 4+ steps | Long workflow with no way to track progress | N/A for simple skills |
| W3 | Feedback/validation loops | Critical operations have validate → fix → repeat cycles | Destructive or quality-critical operations with no validation | N/A if no critical operations |
| W4 | Appropriate patterns used | Patterns match task type (templates for output, examples for demos, conditionals for branches) | Mismatch between task needs and pattern choice | — |
| W5 | Calibrated degrees of freedom | Fragile operations have specific instructions; flexible tasks have high-level guidance | Over-rigid for flexible tasks or too loose for fragile operations | — |
| W6 | Defaults with escape hatches | Recommended approach provided, alternatives noted for edge cases | Multiple equal options presented with no recommendation | — |

---

## 8. Script & Code Checks

*Only evaluate this section if the skill contains a `scripts/` directory.*

| ID | Check | PASS | FAIL | WARN |
|----|-------|------|------|------|
| X1 | Scripts solve, not punt | Scripts handle errors and edge cases internally | Scripts fail silently or throw unhelpful errors for Claude to figure out | — |
| X2 | No magic numbers | Constants are documented with rationale | Unexplained numbers (e.g., `TIMEOUT = 47`) | — |
| X3 | Explicit error handling | Try/except or equivalent with meaningful messages | Bare `open()` or unhandled exceptions | — |
| X4 | Required packages listed | Dependencies documented with install commands | Packages assumed to be available | — |
| X5 | Scripts documented | Purpose and usage described in SKILL.md or inline | Scripts exist with no documentation | — |
| X6 | Forward slashes in paths | All path references use `/` | Backslashes in any path | — |
| X7 | MCP tools fully qualified | MCP tool references use `ServerName:tool_name` format | Bare tool names without server prefix | N/A if no MCP tools |

---

## 9. Quick Pre-Flight Checklist

A condensed pass for rapid validation before upload:

**Structure:**
- [ ] Folder is kebab-case
- [ ] SKILL.md exists (exact case)
- [ ] README.md (if present) is for human install/usage docs only, not skill logic
- [ ] YAML frontmatter has `---` delimiters

**Frontmatter:**
- [ ] `name`: kebab-case, ≤ 64 chars, no reserved words
- [ ] `description`: non-empty, ≤ 1024 chars, third person
- [ ] Description includes WHAT + WHEN

**Content:**
- [ ] SKILL.md body under 500 lines
- [ ] Instructions are specific and actionable
- [ ] Error handling documented
- [ ] Examples provided
- [ ] References linked with context

**Workflow:**
- [ ] Complex tasks have numbered steps
- [ ] Progress checklist for multi-step workflows
- [ ] Validation loops for critical operations
