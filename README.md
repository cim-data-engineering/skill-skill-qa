# skill-qa

Reviews Claude Code skills against best practices and produces a graded QA report.

## Install

### Option 1: Claude Code (CLI)

```bash
git clone https://github.com/cim-data-engineering/skill-skill-qa.git ~/.claude/skills/skill-qa
```

### Option 2: Claude Desktop

1. Click **Code > Download ZIP** from the GitHub repo
2. In Claude Desktop, go to **Settings > Customise > Skills**
3. Upload the ZIP file as a skill

## Usage

From any Claude Code session, run:

```
/skill-qa
```

You'll be prompted to select which skill to review. To review a specific skill, provide its path:

```
/skill-qa path/to/skill
```
