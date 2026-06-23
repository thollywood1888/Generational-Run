# Coaching OS — Agent Prompts
Pre-written prompts for each type of task.
Copy the relevant agent prompt into Claude Code to start a session.

---

## AGENT 0 — SESSION STARTER
Run at the very start of every session (~30 seconds). Establishes a known-good baseline before any edits.

  ---
  Working file: /Users/hollywood/Desktop/Coaching_OS_LIVE/coaching_os.html

  Read CLAUDE.md at /Users/hollywood/Desktop/Coaching_OS_LIVE/CLAUDE.md first.
  Read TASKS.md at /Users/hollywood/Desktop/Coaching_OS_LIVE/TASKS.md to see current status.

  Run the audit from CLAUDE.md before touching anything:
  - How many times does `function go(` appear (must be 1)
  - How many times does `function render()` appear (must be 1)
  - How many times does `function renderDashboard` appear (must be 1)
  - Any getElementById calls referencing IDs that don't exist in renderShell
  - Any render() or go() calls inside a render function (loop risk)

  Report findings. Do not edit anything until audit is complete.
  Then tell me what is broken and what is next based on TASKS.md.
  ---

---

## AGENT 1 — AUDIT AGENT
Use at the start of any session before touching the file.

  Read CLAUDE.md first.
  Working file: /Users/hollywood/Desktop/Coaching_OS_LIVE/coaching_os.html

  Run the full audit from CLAUDE.md (BEFORE EVERY EDIT section).
  Report:
  - How many showSection() definitions exist (must be 1)
  - How many nav click listeners exist (must be 1)
  - Every data-section with no matching data-area
  - Every data-area with no matching data-section
  - Every getElementById referencing a non-existent ID
  - Any render/init call inside a render function (loop risk)

  Do not fix anything. Report only.
  Update TASKS.md status based on findings.

---

## AGENT 2 — NAV FIX AGENT
Use when navigation is broken.

  Read CLAUDE.md first.
  Working file: /Users/hollywood/Desktop/Coaching_OS_LIVE/coaching_os.html

  Run Audit Agent first and paste the report here before fixing.

  Then apply exactly this navigation system and nothing else:
  [paste FIX 2 block from the audit prompt]

  Verify every data-section has a matching data-area.
  Add missing data-area wrappers where needed.
  Open in browser. Confirm all 13 nav items switch correctly.
  Update TASKS.md when done.

---

## AGENT 3 — GENERATE SESSION AGENT
Use when Workout Generator is broken.

  Read CLAUDE.md first.
  Working file: /Users/hollywood/Desktop/Coaching_OS_LIVE/coaching_os.html

  Trace the generate session chain:
  Trigger button → function → inputs → filter APP_DATA.drills →
  output container → Save to Log → state.logs → showSection('log')

  Report where it breaks. Fix only that step.

  Generate flow spec:
  1. Inputs: sessionType, duration, bodyFocus, equipment
  2. Filter APP_DATA.drills matching type + equipment
  3. Shuffle, take 8-12 drills
  4. Render as table into id="generatorOutput"
  5. Show Save to Log and Save to Programme buttons
  6. Save to Log → push to state.logs → saveState() → showSection('log')

  Test the full flow in browser before closing.
  Update TASKS.md when done.

---

## AGENT 4 — FEATURE BUILD AGENT
Use when adding new features.

  Read CLAUDE.md first. Read TASKS.md to confirm feature is QUEUED.
  Working file: /Users/hollywood/Desktop/Coaching_OS_LIVE/coaching_os.html

  Run Audit Agent before starting. Fix any BROKEN items first.
  Do not build new features onto a broken foundation.

  Feature to build: [INSERT FEATURE NAME]

  Rules:
  - New sections need both a data-section nav item AND a data-area wrapper
  - Add section to renderSection() map in CLAUDE.md section map
  - New render functions must be pure (no internal render/showSection calls)
  - New DOM IDs must be added to PROTECTED IDs in CLAUDE.md
  - Test in browser before marking done
  - Update TASKS.md status to 🟢 when verified

---

## AGENT 5 — CSS CLEAN AGENT
Use only when JS is fully stable.

  Read CLAUDE.md first.
  Working file: /Users/hollywood/Desktop/Coaching_OS_LIVE/coaching_os.html

  Audit CSS:
  - Count !important declarations (target: 0)
  - Find any colour values that are not #e8462a red-orange accent
    but should be (buttons, active states, highlights)
  - Find any hardcoded pixel values that should be CSS variables

  Then:
  1. Replace all accent colour variants with var(--accent) = #e8462a
  2. Remove all !important declarations (fix specificity instead)
  3. Ensure :root has one clean token set

  Do not touch JS. Do not touch HTML structure.
  Visual output must be identical before and after.
  Update TASKS.md when done.

---

## AGENT 7 — EXCEL DRILL IMPORT AGENT
Use when you have a new .xlsx file of drills to inject into APP_DATA.

  Working file: /Users/hollywood/Desktop/Coaching_OS_LIVE/coaching_os.html
  Source Excel: /Users/hollywood/Desktop/Coaching_OS_LIVE/source_data/<filename>.xlsx

  Pre-conditions (check before running):
  - The HTML file passes the AGENT 1 audit (no structural issues)
  - The xlsx has an "APP EXPORT" sheet with columns:
    id, name, bodyArea, equipment, trainingType, level, setsTime, how, progression, regression

  Steps:

  1. Run this Python script to inject new drills (edit XLSX_PATH as needed):

  ```python
  import pandas as pd, json, re

  HTML_PATH = '/Users/hollywood/Desktop/Coaching_OS_LIVE/coaching_os.html'
  XLSX_PATH = '/Users/hollywood/Desktop/Coaching_OS_LIVE/source_data/<filename>.xlsx'

  df = pd.read_excel(XLSX_PATH, sheet_name='APP EXPORT')
  df.columns = [c.strip() for c in df.columns]
  df = df.where(pd.notna(df), None)

  with open(HTML_PATH, 'r', encoding='utf-8') as f:
      content = f.read()

  # Extract existing drill names for dedup
  existing = set(re.findall(r'"name":\s*"([^"]+)"', content))
  print(f'Existing names: {len(existing)}')

  # Build new drill objects
  new_drills = []
  skipped = 0
  for _, row in df.iterrows():
      name = str(row.get('name') or '').strip()
      if not name or name in existing:
          skipped += 1
          continue
      sets_raw = str(row.get('setsTime') or '')
      parts = [p.strip() for p in sets_raw.split('·')]
      sets_val = parts[0] if parts else 'Not extracted'
      reps_val = parts[1] if len(parts) > 1 else 'Not extracted'
      drill = {
          "id": str(row.get('id') or name[:8]).strip(),
          "name": name,
          "category": str(row.get('trainingType') or 'General').strip(),
          "bodyPart": str(row.get('bodyArea') or 'Full Body').strip(),
          "equipment": str(row.get('equipment') or 'None').strip(),
          "level": str(row.get('level') or 'Intermediate').strip(),
          "sets": sets_val,
          "repsOrTime": reps_val,
          "rest": "60 sec",
          "coachingCue": str(row.get('how') or 'Not extracted').strip(),
          "progression": str(row.get('progression') or 'Not extracted').strip(),
          "regression": str(row.get('regression') or 'Not extracted').strip()
      }
      new_drills.append(drill)
      existing.add(name)

  print(f'New drills: {len(new_drills)} | Skipped: {skipped}')

  # Locate end of uniqueDrills array
  m = re.search(r'"uniqueDrills":\s*\[', content)
  start = m.end()
  depth = 1; i = start
  while i < len(content) and depth > 0:
      if content[i] == '[': depth += 1
      elif content[i] == ']': depth -= 1
      i += 1
  end = i - 1  # points at closing ]

  # Inject before closing ]
  injection = ',\n' + ',\n'.join(json.dumps(d, ensure_ascii=False) for d in new_drills)
  new_content = content[:end] + injection + content[end:]

  # Update counts
  old_count = int(re.search(r'"totalDrills":\s*(\d+)', new_content).group(1))
  new_total = old_count + len(new_drills)
  new_content = re.sub(r'"totalDrills":\s*\d+', f'"totalDrills": {new_total}', new_content)

  old_unique = int(re.search(r'"uniqueDrills_count":\s*(\d+)', new_content).group(1)) if re.search(r'"uniqueDrills_count":\s*(\d+)', new_content) else 0
  if old_unique:
      new_content = re.sub(r'"uniqueDrills_count":\s*\d+', f'"uniqueDrills_count": {old_unique + len(new_drills)}', new_content)

  with open(HTML_PATH, 'w', encoding='utf-8') as f:
      f.write(new_content)
  print(f'Done. totalDrills: {old_count} → {new_total}')
  ```

  2. After script completes, run sanity check:
     - `grep -o '"totalDrills": [0-9]*' coaching_os.html` — confirm new count
     - Python drill-count script (count { objects in uniqueDrills array)
     - `grep -c "function go(" coaching_os.html` — must be 1
     - `grep -c "function render()" coaching_os.html` — must be 1

  3. Open in browser — verify Exercise Library renders drills from the new source.

  4. Commit and push:
     git add coaching_os.html
     git commit -m "Import N new drills from <filename>.xlsx"
     git push

  5. Update TASKS.md — SESSION LOG entry with drill counts before/after.

---

## AGENT 6 — CHECKPOINT AGENT
Use at end of a productive session to save state.

  Working file: /Users/hollywood/Desktop/Coaching_OS_LIVE/coaching_os.html

  1. Extract the current APP_DATA JSON from the HTML and overwrite
     /Users/hollywood/Desktop/Coaching_OS_LIVE/data_export.json

  2. Create a dated backup:
     cp coaching_os.html backups/coaching_os_[YYYY-MM-DD].html
     (create backups/ folder if it doesn't exist)

  3. Update TASKS.md:
     - Mark completed items as 🟢
     - Add session entry to SESSION LOG with today's date and summary
     - Move any new broken items to CRITICAL

  4. Update CLAUDE.md:
     - Add any new protected IDs
     - Add any new fixed items to WHAT HAS BEEN FIXED
     - Update version history

  5. Report: what is stable, what is still broken, what is next.
