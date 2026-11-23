---
name: design-craft
description: Process for producing UI/UX that looks designed, not AI-generated — for any platform (web, landing page, dashboard, iOS, Android, Ionic/React Native/Flutter, desktop) and any project state (new product, existing product with a design system, existing product with messy ad-hoc styles). Use whenever creating, redesigning, restyling, polishing, or reviewing any screen, page, component, flow, or visual style, including vague asks like "make it look better", "make it modern", "this looks generic", "build a landing page", "design the settings screen". Defers to a project's own design-system skill or DESIGN.md when one exists.
---

# design-craft

AI design looks generic because the model outputs the *average* of its training data whenever an input is unspecified. Banning the average (no purple gradient, no Inter) just moves it to the second-most-common default. The fix is not more bans. It is:

1. **Specific inputs**: real user, real job, real content, real constraints.
2. **Committed decisions**: a direction and a token system chosen *before* screens.
3. **Looking at the result**: render it, screenshot it, critique it, fix it. At least one loop.

Every step below exists to remove one place where the model would otherwise fill in the average. Do not skip steps because the task looks small; scale them down instead (a single component gets a 2-line brief, not a page).

---

## Step 0: Detect the mode

Search before designing:
- Project design-system skill, `DESIGN.md`, `docs/*design*`, Storybook.
- Token sources: `tailwind.config.*`, `theme.*`, `variables.(s)css`, `tokens.json`, `:root { --` blocks, Ionic `theme/`, `Theme.kt` / `Color.kt`, `Colors.swift` / asset catalogs, Flutter `ThemeData`.
- Shared component folders (`components/`, `ui/`, `shared/`).

| Mode | Condition | Your job |
|---|---|---|
| **A. Extend** | A design system exists (skill, DESIGN.md, or consistent tokens + components) | Match it exactly. It is the source of truth and overrides this skill's taste opinions. Originality here = fit, not novelty. New token or component only when nothing existing fits; add it to the system file, never inline. |
| **B. Consolidate** | Existing product, styles ad hoc/inconsistent | Extract what is actually used (recipe below), pick the dominant values as the system, then design with that. Do not invent a fresh look unless asked to redesign. |
| **C. Direct** | New product, or explicit redesign | Run Steps 1-3 fully, including divergent directions. |

**Extraction recipe (Mode B)**, adapt globs to the stack:
```bash
grep -rhoE '#[0-9a-fA-F]{3,8}\b|rgba?\([^)]*\)' src | sort | uniq -c | sort -rn | head -40   # colors
grep -rhoE 'font-size:\s*[^;]+|text-(xs|sm|base|lg|[0-9]xl)' src | sort | uniq -c | sort -rn  # type sizes
grep -rhoE 'border-radius:\s*[^;]+|rounded(-[a-z0-9]+)?' src | sort | uniq -c | sort -rn      # radii
grep -rhoE '(padding|margin|gap):\s*[^;]+' src | sort | uniq -c | sort -rn | head -30         # spacing
```
Cluster near-duplicates (`#1e88e5` vs `#1f89e6` = one color), keep the most-used, write them as tokens, list what gets migrated.

## Step 1: Brief (always, written, before any markup)

Write these five lines into your working notes (and into the reply for non-trivial work):

1. **Who and where**: the person, their device, their context of use. ("Field technician, Android mid-range, outdoors in sun, gloves, hurrying" is a brief. "Users" is not.)
2. **Job of this screen**: the one decision or action the user comes here for. Exactly one primary action.
3. **Real content**: pull actual strings and data from the API, DB, fixtures, existing screens, or the user. List the longest realistic values (names, numbers, translated strings). No lorem, no "John Doe".
4. **Constraints**: platform(s), brand, accessibility needs, density, offline, performance, language/locale.
5. **3-second test**: what the user must understand within 3 seconds of seeing the screen.

If items 1-2 cannot be inferred from the code and conversation, ask **one** decisive question, then proceed. Never dump a questionnaire.

## Step 2: Direction (Mode C, or when a redesign is requested)

**Anchor in the domain, not in adjectives.** "Modern, clean, minimal" produces the average. Instead name 3 concrete nouns from the product's world, and derive visual decisions from them:
- Power-plant safety app → hi-vis signage, permit tags, control-room panels → high-contrast status colors, condensed labels, tag-shaped chips, dense readable tables.
- Neighborhood bakery → paper bags, chalkboard menus, flour → warm off-white ground, hand-set serif headings, kraft accent, generous photography.

**Generate 2-3 divergent directions**, each as a 4-line spec:
```
Direction X: <name from the domain anchor>
Type: <display + text families and why they fit>
Color: <ground / ink / primary / accent roles, with hex>
Shape & density: <radius, borders vs shadows, compact vs airy>
Signature move: <the ONE memorable, repeatable thing>
```
They must differ on at least two axes (e.g. serif vs grotesk, dense vs airy, light vs dark ground). Discard the one closest to "default SaaS template". Pick one with a one-line reason, or let the user pick if they are present and the stakes are high.

**One signature move**: one specific, repeated gesture that makes the product recognizable (a tag-shaped chip, a typographic treatment for numbers, a distinctive section divider, a particular image crop). One. Applied consistently. Zero is sterile; five is noise.

Visual references (screenshots the user provides, a URL to study) are inputs for *principles* (density, rhythm, type contrast), never templates to clone.

## Step 3: Tokens before screens

Write or confirm the system file first (CSS variables, Tailwind theme, Ionic theme, Compose/SwiftUI theme). Minimum:

- **Color roles**, not raw colors: background, surface, surface-raised, text, text-muted, border, primary, on-primary, accent, success, warning, danger, info. Plus dark values if dark mode ships. Muted text still passes 4.5:1.
- **Type scale**: 5-7 steps with explicit size / line-height / weight / tracking. Real contrast between steps (heading vs body at least ~1.25x per step); one display treatment.
- **Spacing scale**: 4 or 8 base (4, 8, 12, 16, 24, 32, 48, 64). Use it to *group* (tight inside a group, loose between groups), never one value everywhere.
- **Radius**: 2-3 values with assigned roles (e.g. controls 8, cards 12, sheets 20). Not everything pill.
- **Elevation**: pick one primary separation strategy (borders OR shadows OR tonal surfaces) and use the others sparingly.
- **Motion**: 2-3 durations and one easing curve; purpose for each use.

From here on, components use tokens only. A hard-coded hex or px in a component is a bug.

## Step 4: Compose

- **Hierarchy**: one focal point per screen. Squint at it: the primary action and the most important information must still read first.
- **Layout from content**: structure follows what the content needs and the order the user thinks in, not a template. Landing: not automatically hero → 3 features → testimonials → pricing → FAQ. Dashboard: not automatically sidebar + 4 stat cards + chart + table; lead with whatever drives the user's decision.
- **Density matches the job**: operational/enterprise tools are dense and scannable; consumer/marketing pages breathe. Don't pad an ops table into a landing page.
- **Platform conventions**: read `references/platforms.md` for the target platform before building. A native-feeling screen beats a "creative" one that fights the OS.
- **States are part of the design**, not extras: loading (skeleton matching real layout), empty (why + the one action that fills it), error (what failed + recovery), partial/offline, no permission, and content extremes (0, 1, many items; very long name; huge number; missing image).
- **Copy is design**: specific verbs on buttons ("Kirim laporan", "Retry sync", not "Submit"/"Get Started"), the user's language consistently (no mixed Indonesian/English UI unless the product does that on purpose), no buzzwords, no em-dash-laden marketing voice.
- **Honesty**: never invent stats, testimonials, logos, people, or metrics. Unknown values become clearly labeled placeholders (`[angka asli]`, initials avatar, "Coming soon").
- **Icons**: one set, one stroke weight, one size grid; an icon only where it speeds recognition. No sparkles/rockets/robots as decoration. No emoji in UI chrome.
- **Accessibility baseline**: contrast AA, touch targets ≥ 44pt/48dp, visible focus, labels on fields and icon buttons, respects reduced motion and dynamic type/zoom.

## Step 5: Look at it (non-negotiable)

Code that has not been seen has not been designed. Render and screenshot at real sizes, then critique with `references/critique.md`.

Ways to get pixels, use whatever exists in the session:
- Browser automation tools (Playwright/Chrome MCP, built-in browser) if available.
- `npx playwright screenshot --viewport-size=390,844 http://localhost:PORT/route mobile.png` and `--viewport-size=1440,900` for desktop. Add `--color-scheme=dark` for dark mode. Works on `file://` paths too. If it fails with a missing-browser error, run `npx -y playwright install chromium` once.
- iOS simulator: `xcrun simctl io booted screenshot shot.png`. Android: `adb exec-out screencap -p > shot.png`.
- Then **Read the PNG** so you actually see it.

Loop: screenshot → score with the rubric → fix the top 3 problems → screenshot again. Minimum one full loop; stop when the rubric has no 1s and the 3-second test passes. Check at least: smallest supported mobile width, a desktop/tablet width (if supported), dark mode (if shipped), and one empty/error state.

If rendering is truly impossible (no dev server, no simulator), say so explicitly in the reply and critique by code reading. Never claim it was visually verified when it was not.

## Step 6: Report

Keep it short:
- Brief (5 lines) and chosen direction (Mode C) or system used (Mode A/B).
- Tokens added or changed.
- Screenshot paths, and the critique loop: what scored low, what you fixed.
- Known remaining issues and placeholders that need real data.

---

## Tells: if you see one, ask "what does this serve?"

Not a ban list. Any of these is fine *with a reason rooted in the brief*. Several together without reasons = the average showed up.

- Purple/blue/cyan gradient, glowing orbs, mesh blobs, dark-mode-because-tech.
- Default font with no reason (Inter/Geist/Space Grotesk/Poppins everywhere), wide-tracked uppercase eyebrows over every section.
- Pill badge above the H1 repeating the headline; "New"/"AI-powered" capsules.
- Identical cards in a 3-up grid with an icon in a tinted circle; bento grid as filler.
- Every element rounded the same, every element shadowed, glassmorphism on everything.
- Stat cards with invented numbers and "+12%" deltas; fake logo bars; fake testimonials.
- "Get Started" / "Learn More" CTAs with an arrow on every button.
- Colored left-border stripe on cards as decoration.
- Uniform spacing everywhere, centered everything, sections that differ only by background color.
- Emoji as icons, sparkles as the AI icon, Undraw-style illustration.
- Web-isms in mobile apps: hover-dependent UI, tiny text links as primary actions, desktop modals instead of sheets, no safe-area handling.
- Happy-path only: no empty/error/loading state, content that only works with short English strings.
