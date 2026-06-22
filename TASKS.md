# Coaching OS — Task Navigator
Update this file after every session.

## STATUS KEY
🔴 BROKEN   — not working, needs fix
🟡 PARTIAL  — works but incomplete
🟢 DONE     — verified working in browser
⬜ QUEUED   — not started

---

## CRITICAL (fix before anything else)
🟢 Navigation click routing — all 13 items verified, active state correct (verified)
🟢 Generate Session — null-safety fixed, try-catch added (verified)
🟢 Save to Log — saveGenerated() writes Planned entry to state.logs (verified)

## CORE FEATURES
🟢 Dashboard — redesigned to match mockup (3-col + weekly grid, verified)
🟢 Today page — energy/difficulty buttons wired
🟢 Training Log — search + filter working
🟢 Athlete Profile — save and delete wired
🟢 Targets — progress bars rendering
🟡 Progress tabs — 7 tabs wired / data may be placeholder
⬜ Programme Builder — create and edit full programme
🟢 Workout Generator — full generate → save flow (verified)
⬜ Exercise Library — browse and filter 1,797 drills
⬜ Imported Packs — display xlsx-sourced drill packs
⬜ Export — export state as JSON / PDF

## DESIGN
🟡 Sidebar nav — structure correct / active state unreliable
⬜ Mobile responsive layout
⬜ Dark/light theme toggle
⬜ Accent colour consistency audit (#e8462a throughout)

## DATA
🟢 APP_DATA embedded — 1,797 drills, 2 athletes
🟢 data_export.json — standalone backup
⬜ Import flow — load new drills from xlsx without rebuilding HTML
⬜ Multi-athlete data isolation

## SESSION LOG
2026-06-22 — File reorganised to Coaching_OS_LIVE/, all stale versions trashed
2026-06-22 — 21 checks passed on tab/button fixes
2026-06-22 — Navigation jumpiness fixed (scroll reset + sidebar rebuild optimisation)
2026-06-22 — Generate session null-safety fixed, try-catch added
2026-06-22 — CLAUDE.md, TASKS.md, AGENTS.md created
2026-06-22 — saveGenerated() fixed: now pushes Planned entry to state.logs + savedSessions
2026-06-22 — null-safety audit: loadState deep-merges filters; renderGenerator, renderLog, recentRows guards added
2026-06-22 — Dashboard redesigned: 3-col layout (session + stats + workout summary) + 7-day week grid, matches mockup (browser verified)
2026-06-22 — Generator: preview/save split, Not extracted fallbacks fixed, full flow verified
