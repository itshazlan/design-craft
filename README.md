# design-craft

An agent skill for producing UI/UX that looks designed, not AI-generated. Works for web, mobile (iOS, Android, Ionic/React Native/Flutter), and desktop, on new or existing projects.

Instead of a ban list, it gives the agent a process:

1. **Detect the mode**: extend an existing design system, consolidate ad-hoc styles, or set a new direction.
2. **Brief**: real user, the screen's job, real content, constraints, 3-second test.
3. **Direction**: anchored in the product's domain, 2-3 divergent options, one signature move.
4. **Tokens before screens**.
5. **Compose** with platform conventions and all UI states.
6. **Look at it**: screenshot, critique with a rubric, fix, repeat.

## Files

- `SKILL.md`: the workflow
- `references/critique.md`: screenshot critique rubric and stress tests
- `references/platforms.md`: web, iOS, Android, hybrid, desktop, enterprise conventions

## Install

Clone into the skills folder your agent reads, or clone once and symlink:

```bash
git clone https://github.com/itshazlan/design-craft ~/.claude/skills/design-craft

ln -s ~/.claude/skills/design-craft ~/.agents/skills/design-craft           # Codex, Pi, OpenCode
ln -s ~/.claude/skills/design-craft ~/.gemini/config/skills/design-craft    # Antigravity
```

OpenCode also reads `~/.claude/skills/` directly.

## Use

It triggers automatically on UI work. To force it:

- Claude Code: `/design-craft <task>`
- Pi: `/skill:design-craft <task>`
- Others: "Use the design-craft skill. <task>"
