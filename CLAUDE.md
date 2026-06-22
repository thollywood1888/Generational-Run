# Coaching OS — Claude Governance Document
Last updated: 2026-06-22 (checkpoint 2)
Version: coaching_os.html (single file, no splits)

## PROJECT IDENTITY
Single-file HTML coaching application.
Master file: coaching_os.html
All edits go to this file only. No new HTML files. No splitting.

## CURRENT STATE
- Navigation: data-section → showSection() → data-area routing
- Accent colour: #e8462a red-orange
- Sidebar: fixed 200px dark navy, .v5-nav
- Data: APP_DATA object embedded in the HTML (1,797 drills, 2 athletes)
- State: localStorage keys — active_section, coaching_os_state
- CSS: single :root token set (no !important, no layered patches)

## ARCHITECTURE RULES — NEVER VIOLATE
1. ONE showSection() function only — defined once, never overridden
2. ONE click listener for nav — the document.addEventListener pattern
3. ONE renderSection() router — maps section IDs to render functions
4. Render functions are PURE — they read state and write HTML.
   They never call render(), showSection(), or init() internally.
5. No setInterval or setTimeout calling render or showSection
6. Every getElementById and querySelector must be null-checked:
   const el = document.getElementById('x'); if(!el) return;
7. No !important in CSS — ever

## SECTION MAP
Every data-section value must have a matching data-area wrapper:
  dashboard   → [data-area="dashboard"]
  today       → [data-area="today"]
  programme   → [data-area="programme"]
  create      → [data-area="create"]
  generator   → [data-area="generator"]
  library     → [data-area="library"]
  packs       → [data-area="packs"]
  progress    → [data-area="progress"]
  log         → [data-area="log"]
  profile     → [data-area="profile"]
  targets     → [data-area="targets"]
  settings    → [data-area="settings"]
  export      → [data-area="export"]


## BEFORE EVERY EDIT — RUN THIS CHECK
grep -n "function showSection" coaching_os.html | wc -l  → must be 1
grep -n "addEventListener.*click" coaching_os.html | wc -l → must be 1
grep -n "data-section=" coaching_os.html | sort -u
grep -n "data-area=" coaching_os.html | sort -u
→ Every data-section must have a matching data-area. Fix mismatches first.

## AFTER EVERY EDIT — VERIFY IN BROWSER
□ No console errors on load
□ All sidebar nav items switch to correct section
□ Generate → drills appear → Save to Log → log entry appears
□ Reload restores last section
□ CPU stays normal (no loop)

## PROTECTED IDs — DO NOT RENAME OR REMOVE
  id="topbar"           — topbar header element
  id="sidebar"          — sidebar nav element
  id="content"          — main content section
  id="toast"            — toast notification div

## WHAT HAS BEEN FIXED (DO NOT REVERT)
- All V3/V4 CSS patches consolidated into single :root
- showSection() deduplicated to single version
- Null guards added to quickGenerate, quickProgramme, quickLogRun,
  quickExport, sidebarAthlete
- state.logQuery, state.logTypeFilter, state.logAthFilter added
- renderLog() rebuilt with live search and filter
- renderTargets() rebuilt with progress bars
- All Progress page tabs wired (7 tabs)
- Athlete Profile Save and Delete wired
- Today page energy/difficulty buttons wired
- generatedSession() null-safe fallback (no more equipment.includes TypeError)
- saveGenerated() wrapped with try-catch (errors now surface as toast)
- renderShell() optimised — sidebar only rebuilt on first load or athlete change
- Scroll-to-top added on every page navigation
- loadState() deep-merges filters sub-object to prevent schema-drift nulls
- renderGenerator, renderLog, recentRows null-guarded (||'' on all field accesses)
- Dashboard redesigned: 3-col layout (today's session + weekly stats + workout summary) + 7-day week grid

## VERSION HISTORY
v4 — Complete_Training_OS_v4_Premium_Coach_UI.html (TRASHED)
v5 — Personal_Coaching_OS_Red_Performance_Edition.html (RENAMED)
LIVE — coaching_os.html ← THIS IS THE ONLY FILE
