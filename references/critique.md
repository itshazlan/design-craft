# Critique rubric

Use on a real screenshot, not on code. Score each row 1-3. Any 1 must be fixed before delivery. Write the scores down; a vague "looks good" is not a critique.

| # | Dimension | 1 (fix) | 3 (good) | How to check |
|---|---|---|---|---|
| 1 | **3-second read** | Eye wanders; primary action not obvious | The brief's 3-second message lands immediately | Look for 3 seconds, look away, state what you saw |
| 2 | **Hierarchy** | Several elements compete at the same weight | One clear focal point, then a clear second level | Squint/blur the screenshot: does the order survive? |
| 3 | **Grouping & spacing** | Even gaps everywhere; unclear what belongs together | Tight within groups, loose between; aligned edges | Trace left edges; check that related items are closer than unrelated |
| 4 | **Type** | Sizes too close together, too many sizes/weights, cramped line-height | Distinct scale steps, comfortable measure (45-75 chars on web), consistent | Count distinct sizes on screen (aim ≤ 4) |
| 5 | **Color** | Accent everywhere, low contrast, color is the only state signal | Accent reserved for key moments; AA contrast; states also use shape/text | Check muted text and text on colored fills specifically |
| 6 | **Identity (swap test)** | Swap logo/name and it could be any product | Domain anchor and signature move visible | Would a competitor's logo fit here unchanged? |
| 7 | **Platform fit** | Feels like a web page inside an app (or a phone UI stretched on desktop) | Navigation, controls, gestures, and spacing match the platform | Compare against `platforms.md` for the target |
| 8 | **Content stress** | Breaks with long names, big numbers, empty lists, translated strings | Truncates/wraps gracefully; empty and error states designed | Re-render with the longest real values and with zero items |
| 9 | **Touch & reach** (mobile) | Targets small/crowded; primary action in a hard-to-reach corner | ≥ 44pt/48dp, primary actions in thumb zone, no hover dependence | Overlay a 44px grid mentally on the screenshot |
| 10 | **Consistency** (Mode A/B) | New one-off colors, radii, components | Only system tokens and existing components | Grep the diff for raw hex/px values |

## Common fixes, in the order they usually pay off

1. **Remove** before adding: delete decorative elements that serve no row above.
2. **Raise contrast between hierarchy levels**: bigger/bolder primary, quieter secondary (muted color, smaller size), not more boxes.
3. **Regroup with spacing**, then remove borders/cards that were only doing grouping.
4. **Reduce accent usage** to the primary action and one signature place.
5. **Fix the content extremes** (truncation, wrapping, empty states).
6. **Add the signature move** only after the above are solid.

## Stress-test checklist

- Longest realistic name/title (and its translation if the app is multilingual; Indonesian strings run ~20-30% longer than English).
- Number formatting: thousands separators, currency, units, negative values, 0.
- List with 0, 1, and 100+ items.
- Missing image / avatar / optional field.
- Slow network: what shows during loading? Offline?
- Dark mode, large text / dynamic type, smallest supported width (320-360px).
