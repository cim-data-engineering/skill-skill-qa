# Anti-Patterns Catalog

Common mistakes in skill authoring. Use this as a quick reference when reviewing skills — if you spot any of these patterns, flag them.

## Contents

1. [Structural Anti-Patterns](#1-structural-anti-patterns)
2. [Description Anti-Patterns](#2-description-anti-patterns)
3. [Content Anti-Patterns](#3-content-anti-patterns)
4. [Workflow Anti-Patterns](#4-workflow-anti-patterns)
5. [Code Anti-Patterns](#5-code-anti-patterns)

---

## 1. Structural Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|---|---|---|
| README.md duplicating SKILL.md content | Claude may load conflicting instructions from both files | Keep README.md for human-facing install/usage docs only; all skill logic belongs in SKILL.md |
| Deeply nested references (A → B → C) | Claude may only partially read files discovered through chained references | Flatten: SKILL.md links directly to each reference file |
| Vague folder/file names (`helpers/`, `utils.md`, `tools/`) | Hard to understand purpose from the name alone | Use descriptive names (`validation-rules.md`, `api-client.py`) |
| Windows-style paths (`scripts\helper.py`) | Breaks on macOS/Linux; inconsistent cross-platform behavior | Use forward slashes everywhere (`scripts/helper.py`) |
| Non-kebab-case folder name (`My_Skill`, `mySkill`) | Violates naming convention; may cause upload errors | Use kebab-case: `my-skill` |
| SKILL.md filename variations (`skill.md`, `SKILL.MD`) | Claude and the platform require exactly `SKILL.md` | Rename to exactly `SKILL.md` |

---

## 2. Description Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|---|---|---|
| First person ("I can extract...", "I will help you...") | Description is injected into the system prompt; inconsistent POV harms discovery | Rewrite in third person ("Extracts...", "Generates...") |
| Too vague ("Helps with projects", "Processes data") | Claude cannot determine when to trigger the skill | Be specific: what domain, what actions, what file types |
| Missing trigger phrases | Skill may never activate automatically | Add "Use when..." with specific phrases users would say |
| Only formal triggers | Misses casual user phrasing, causing under-triggering | Add natural variations ("help me with...", "can you...") |
| Too broad scope | Skill triggers on unrelated queries (over-triggering) | Narrow the domain; add negative triggers if needed |
| Reserved words in name ("claude-helper", "anthropic-tools") | Rejected by the platform | Remove "claude" and "anthropic" from the name |
| XML tags in frontmatter (`<br>`, `<em>`) | Security restriction — frontmatter appears in system prompt | Remove all `<` and `>` characters from frontmatter |

---

## 3. Content Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|---|---|---|
| Explaining what Claude already knows | Wastes tokens on obvious context (what a PDF is, how JSON works) | Remove — assume Claude knows common concepts |
| Time-sensitive content ("As of March 2025...") | Becomes stale and misleading over time | Remove dates; use "current method" / "legacy method" sections |
| Inconsistent terminology | Confuses Claude; same concept called different things | Pick one term per concept and use it consistently |
| Abstract instructions ("validate properly", "handle errors") | Not actionable — Claude cannot determine what "properly" means | Be specific: what to validate, what errors, what recovery steps |
| No examples | Claude has to guess the expected format and quality | Add concrete input/output examples or code snippets |
| Excessive ALL-CAPS emphasis | Loses impact when overused; feels like shouting | Reserve emphasis for truly critical safety or correctness rules |
| Inline reference data exceeding 100 lines | Bloats SKILL.md; loads unnecessary context | Move to a reference file and link from SKILL.md |

---

## 4. Workflow Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|---|---|---|
| Too many options without a default | Claude may pick suboptimally or ask unnecessary questions | Recommend one approach; mention alternatives as escape hatches |
| No error handling for failure modes | Claude is stuck when operations fail | Document common failures and recovery steps |
| Missing validation/feedback loops | Errors in output go undetected until too late | Add validate → fix → repeat cycles for critical operations |
| Over-rigid for flexible tasks | Unnecessary constraints on tasks where multiple approaches work | Use high-freedom instructions; let Claude choose the approach |
| Under-specified for fragile tasks | Dangerous operations without guardrails | Add specific step-by-step instructions with validation gates |
| Complex workflow as prose | Hard to follow; Claude may skip steps | Break into numbered steps with a progress checklist |

---

## 5. Code Anti-Patterns

| Anti-Pattern | Why It's Bad | Fix |
|---|---|---|
| Magic numbers (`TIMEOUT = 47`) | Claude cannot judge if the value is correct or needs adjustment | Add a comment explaining the rationale for each constant |
| Assuming packages installed | Script fails with import errors in clean environments | List dependencies with install commands (`pip install X`) |
| Bare `open()` / unhandled exceptions | Script crashes; Claude gets an unhelpful error message | Add try/except with meaningful error messages and recovery |
| Punting to Claude ("just fail and let Claude figure it out") | Wastes tokens on debugging; non-deterministic outcomes | Handle errors in the script; provide clear error output |
| Bare MCP tool names (`create_issue`) | Tool not found when multiple MCP servers are available | Use fully qualified names (`GitHub:create_issue`) |
| Undocumented scripts | Claude doesn't know when or how to use the script | Document purpose and usage in SKILL.md with example commands |
