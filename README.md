# design-craft

An agent skill for producing UI/UX that looks designed, not AI-generated. Works for web, mobile (iOS, Android, Ionic/React Native/Flutter), and desktop, on new or existing projects.

Instead of a ban list, it gives the agent a process:

1. **Detect the mode**: extend an existing design system, consolidate ad-hoc styles, or set a new direction.
2. **Brief**: real user, the screen's job, real content, constraints, 3-second test.
3. **Direction**: anchored in the product's domain, 2-3 divergent options, one signature move.
4. **Tokens before screens**.
5. **Compose** with platform conventions and all UI states.
6. **Look at it**: screenshot, critique with a rubric, fix, repeat.

## Install

### Option 1: one command (recommended)

Uses the [`skills`](https://skills.sh) CLI, which knows where each agent keeps its skills:

```bash
npx skills add itshazlan/design-craft -g
```

It asks which agents to install to. To install to every supported agent without prompts:

```bash
npx skills add itshazlan/design-craft -g -a '*' -y
```

Drop `-g` to install into the current project only (shared with your team through the repo).

### Option 2: manual

Clone once, then symlink into the folder each agent reads:

```bash
git clone https://github.com/itshazlan/design-craft ~/.claude/skills/design-craft
```

| Agent | Global skills folder | Command |
|---|---|---|
| Claude Code | `~/.claude/skills/` | nothing more, the clone above is enough |
| OpenCode | reads `~/.claude/skills/` and `~/.agents/skills/` | nothing more |
| Codex | `~/.agents/skills/` | `ln -s ~/.claude/skills/design-craft ~/.agents/skills/design-craft` |
| Pi | `~/.agents/skills/` or `~/.pi/agent/skills/` | same as Codex |
| Antigravity (IDE and `agy`) | `~/.gemini/config/skills/` | `ln -s ~/.claude/skills/design-craft ~/.gemini/config/skills/design-craft` |
| Cursor, Zed, or any agent without skill support | none | add the rule in [Agents without skill support](#agents-without-skill-support) |

For one project only, put the folder in the project instead: `.claude/skills/design-craft` (Claude Code) or `.agents/skills/design-craft` (Codex, Pi, OpenCode, Antigravity).

**Restart running agent sessions after installing.** Skills are loaded when a session starts.

### Check it is installed

Ask the agent: *"Is a skill named design-craft available? Answer YES or NO."*

### Update and uninstall

```bash
npx skills update design-craft          # if installed with Option 1
git -C ~/.claude/skills/design-craft pull   # if installed with Option 2

npx skills remove design-craft          # Option 1
rm -rf ~/.claude/skills/design-craft    # Option 2, then delete the symlinks you made
```

## Use

### Automatically

Ask for UI work as usual. The skill loads when the request matches its description, for example:

- "Redesign the order history screen"
- "Build a landing page for this product"
- "This dashboard looks generic, make it better"
- "Review the UI of the settings page"

### Explicitly (recommended for non-Claude models)

Some models do not load skills on their own reliably. Force it:

| Agent | How |
|---|---|
| Claude Code | `/design-craft redesign the order history screen` |
| Pi | `/skill:design-craft redesign the order history screen` |
| Codex, OpenCode, Antigravity, others | start the prompt with "Use the design-craft skill." |

### Agents without skill support

Add this line to the project's `AGENTS.md` (or the agent's rules file):

```
For any UI/UX work, read and follow ~/.claude/skills/design-craft/SKILL.md and its references/ folder.
```

This also helps agents that do support skills but tend to skip them.

### What to expect

For a non-trivial task the agent should:

1. State which mode it detected (extend, consolidate, or direct) and which design system it follows.
2. Write a 5-line brief before any markup.
3. In direct mode, propose 2-3 directions and pick one with a reason.
4. Set or confirm tokens before building screens.
5. Take screenshots, score them with the rubric, fix the low scores, and screenshot again.
6. Finish with a short report: brief, direction, tokens changed, screenshots, what it fixed, what is still open.

If it skips the brief or the screenshots, tell it to follow the skill.

### Requirements for the screenshot step

- A model that can read images. Text-only models fall back to critique by reading the code, and the skill tells them to say so.
- A way to take screenshots: the agent's browser tool, or Playwright (`npx -y playwright install chromium` once), or the iOS simulator / Android `adb`.

### Projects with their own design system

If the project has a design-system skill, `DESIGN.md`, or consistent tokens, design-craft follows that system instead of its own style opinions.

## Files

- `SKILL.md`: the workflow
- `references/critique.md`: screenshot critique rubric and stress tests
- `references/platforms.md`: web, iOS, Android, hybrid, desktop, enterprise conventions

## License

[MIT](LICENSE)
