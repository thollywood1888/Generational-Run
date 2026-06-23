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
🟢 Progress tabs — 7 tabs wired, all real data (consistency%, streak, body-part coverage computed from logs)
🟢 Programme Builder — name, save, load, delete multiple named programmes; library panel in UI
🟢 Workout Generator — full generate → save flow (verified)
🟢 Exercise Library — search, filter, category tabs, detail panel (2,490 drills, verified)
🟢 Imported Packs — all 7 source files displaying with drill/workout row counts (verified)
🟢 Export — Export Full JSON / Training Log / Drill Library buttons wired (verified)

## DESIGN
🟢 Sidebar nav — active state fixed (querySelectorAll runs unconditionally on every render)
🟢 Mobile responsive — block-grid, split, two-col, bodymap, dash-grid, session-hdr all collapse at 800px; section-tabs scroll horizontally
⬜ Dark/light theme toggle
⬜ Accent colour consistency audit (#e8462a throughout)

## DATA
🟢 APP_DATA embedded — 2,490 drills (1,569 unique), 2 athletes
🟢 data_export.json — standalone backup
🟢 Excel import — 693 drills injected from excel master training.xlsx (AGENT 7 prompt created)
🟢 Multi-athlete data isolation — log filter defaults to active athlete; switching athlete resets filter
⬜ Import flow — UI to load new drills from xlsx without rebuilding HTML
⬜ Multi-athlete data isolation (write path) — distance field for Running tab

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
2026-06-22 — Excel import: 693 drills injected from APP EXPORT sheet (uniqueDrills 876→1,569, totalDrills 1,797→2,490), pushed to GitHub
2026-06-23 — Browser audit: Exercise Library, Imported Packs, Export all verified working
2026-06-23 — Sidebar active state fixed: querySelectorAll now unconditional
2026-06-23 — Progress tabs: Overview + Body-Part Coverage now use real computed data (consistency%, streak, avgMins, per-area session counts)
2026-06-23 — Programme Builder: programmeName, save/load/delete library, activePlanDays() routing
2026-06-23 — Mobile responsive: 8 layout classes collapse at 800px; inline grids extracted to CSS classes
2026-06-23 — Multi-athlete isolation: log defaults to active athlete, resets on athlete switch
