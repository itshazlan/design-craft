# Platform conventions

Read only the section(s) for the target. Native-feeling beats novel. Put originality in type, color, content, and the signature move, not in reinventing navigation.

## Web (sites and web apps)

- Breakpoints come from where the content breaks, not from device lists. Design mobile width first, then widen.
- Body text 16px minimum; line length 45-75 characters; never block zoom.
- Hover is an enhancement only; every hover affordance needs a tap/focus equivalent. Visible `:focus-visible` style.
- Navigation reflects pages that exist. On mobile, the nav collapses into a real, labeled trigger.
- Landing pages: order sections by the visitor's questions (What is it? Is it for me? Proof? How much? How do I start?), drop any section you have no real content for.
- Web apps/dashboards: lead with the thing the user acts on (failing items, today's tasks), not with vanity stat rows. Tables: the deciding column first, numbers right-aligned with tabular figures, sticky header for long tables.
- Media sized (`width`/`height` or `aspect-ratio`), hero image not lazy-loaded.

## iOS (native or iOS mode of a hybrid app)

- Navigation: navigation bar with large title on top-level screens collapsing on scroll; tab bar with 3-5 destinations; back by swipe from the left edge must work.
- Modality: sheets (with detents) for focused subtasks, action sheets for choices, alerts only for things that must interrupt.
- Type: system font (SF) or a brand font that respects Dynamic Type sizes; text styles (Large Title, Title, Headline, Body, Footnote, Caption).
- Targets 44×44pt. Respect safe areas (notch, home indicator); no controls under the home indicator.
- Grouped inset lists for settings/forms; swipe actions on list rows for secondary actions.
- Feedback: subtle haptics on meaningful actions, no toast spam.

## Android (native or Material mode of a hybrid app)

- Material 3: top app bar, navigation bar (3-5 destinations) or navigation rail on large screens; system back gesture/button must always do the expected thing.
- FAB only for the screen's single primary *creating* action; not as a generic "+" decoration.
- Bottom sheets for contextual actions, dialogs for confirmations, snackbars for undoable feedback.
- Targets 48×48dp. Edge-to-edge with proper insets for status and navigation bars.
- Tonal surfaces (surface containers) rather than heavy shadows; ripple/state layers on press.
- Respect system font scale; test at 1.3-2.0x.

## Hybrid / cross-platform (Ionic, Capacitor, React Native, Flutter)

- Pick per platform deliberately: either adapt (Ionic `mode`, platform-aware components) or commit to one consistent brand UI across both. Mixed half-iOS/half-Material is the worst option.
- Kill web-isms: hover states, text links as primary actions, browser scrollbars, desktop-style centered modals on phones, 300ms tap delays, focus rings that look like browser defaults.
- Safe areas: `env(safe-area-inset-*)` / `SafeAreaView` / `SafeArea` on headers, footers, sticky buttons, and full-screen sheets.
- Keyboard: inputs must stay visible above the keyboard; sticky footer buttons move with it or hide.
- Lists: virtualize long lists; pull-to-refresh where data is refreshable; skeletons that match the real row layout.
- Offline-first apps: show sync state honestly (pending, synced, failed + retry) as part of the design, not a hidden toast.
- Performance feel: avoid heavy blur/shadow/gradient stacks on mid-range Android; they cost frames.

## Desktop apps (Electron, Tauri, native)

- Respect OS window chrome, menu bar conventions, and keyboard shortcuts (show them in menus/tooltips).
- Higher density than mobile; hover and right-click are legitimate; resizable panes with sensible minimums.

## Enterprise / operational tools (any platform)

- Density and scan speed beat whitespace. Users repeat tasks hundreds of times: optimize for the 100th use, not the first impression.
- Status is the core visual language: define a small, consistent status palette (with icons/text, not color alone) and use it everywhere.
- Forms: group by the user's mental model, smart defaults, inline validation, preserve input on error.
- Outdoor/field use: higher contrast, larger text and targets, no low-contrast muted grays for important data.
