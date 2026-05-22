
# ANSWERS.md

## 1. How to run

No installation required. Open `index.html` directly in any modern browser, or serve it locally:

Data is stored in `localStorage`- no backend or build setup. 
---

Deployed URL: https://pattern-habit.vercel.app/


## 2. Stack & design choices

**Stack:** HTML/CSS/JS- For a habit tracker that needs to persist data locally and run instantly in a browser, a framework would add friction with no payoff.


**Decisions*** 

**- Week-first grid over a list or calendar**
Habits run down the left, days run across the top. This makes the current week scannable in a single horizontal sweep. You can see all your habits and all 7 days simultaneously. A list would require scrolling to compare across days while a calendar would waste space on date metadata. The grid also makes streaks visually obvious- a run of filled cells reads as momentum without needing a number.


**- Streak counts up to today**
If today is unchecked, the streak is 0. This creates real-time feedback pressure, which is the point of a streak counter.

---

## 3. Responsiveness & accessibility

**360px phone:** The habit name column shrinks, the connection note is hidden, and the header stacks vertically. The grid scrolls horizontally. The table has `min-width: 560px` and the wrapper is `overflow-x: auto`, so the grid never breaks but stays navigable. 
Add/domain controls wrap to a second line.

**1440px laptop:** The app is capped at `980px` max-width and centered. The grid has room to breathe, habit names are fully visible, and the tag list under each habit is readable without truncation.

**Accessibility handled:** Focus states are preserved on all interactive elements (checkboxes, buttons, inputs). The habit name is `contenteditable` with keyboard support.
Enter commits the rename. All buttons have visible focus rings from the browser default (not removed via `outline: none` on focus).

**Accessibility skipped:** Each button currently has no text content- a screen reader would read nothing meaningful. With more time I'd add `aria-label="Mark [habit name] for [date]"` dynamically on each check button.

---

## 4. AI usage

Used Claude (Anthropic) throughout. Specific uses:

- **Initial scaffold:** Asked for a dark-mode habit tracker with a weekly grid, streaks, and specific UX considerations for people targeting multiple domains. Added macro categories which can further be customized with specific domains and each assigned a macro category. Got a working single-file HTML — kept the overall structure, rewrote the domain/color system entirely since it was hardcoded to my own domains.

- **Tag/link system:** Asked Claude to guide on adding clickable cross-domain tags that link one habit to another, with optional date pinning and navigation. 

- **Pre-scheduling:** Asked for 3-state toggle (empty-> planned-> done) on future cells. The AI kept the `disabled` attribute on future buttons. I removed it and added the dashed-border planned state so future days are interactable but visually distinct from completed ones.

- **Domain manager:** Asked for a setup modal with 4 macro-categories (Physical, Mental, Spiritual, Accountability) where users define their own domains. The AI hardcoded a color per macro instead of letting users pick per domain. I added a `<input type="color">` per macro section so each domain gets its own color.

---

## 5. Honest gap

The cross-domain connections panel (the "Show" toggle at the bottom) still just lists habit notes as text- it doesn't visualize the tag links as an actual graph or map. You can add tags and click them to jump to linked habits, but there's no bird's-eye view of how your domains connect to each other. 
With another day I'd build a simple force-directed SVG graph (D3 or even hand-rolled) showing habits as nodes colored by macro-category, with edges drawn for each tag link — so you can actually *see* your cross-domain patterns at a glance instead of hunting through rows.

I would also like to add a grid based color-coded review map that builds up during the month and shows the progress throughout the month across different macro-categories and domains within them.
