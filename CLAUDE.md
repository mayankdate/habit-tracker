# HabitTracker: contributor and AI coding guidance

## Purpose

HabitTracker is a personal habit tracker, a static browser app served by GitHub
Pages at https://mayankdate.github.io/habit-tracker/ (branch `main`, folder
`/docs`). Habits and challenges are defined in `docs/habits.yaml`; check-ins are
stored in `docs/checkins.csv` and saved by committing that file to GitHub
directly from the browser. There is no server, no build step and no Notion
dependency (the Notion dashboard, its scripts and its scheduled Action were
removed).

The owner is not a programmer and never uses the terminal; they work in VS Code
tasks and the browser. Keep user-facing instructions in `README.md` plain and
click-by-click.

## Architecture

```
HabitTracker/
├── docs/
│   ├── index.html            ← the whole app: hand-written HTML/JS, edit directly
│   ├── habits.yaml           ← settings: habits, challenges, github save target
│   ├── checkins.csv          ← check-in data, one row per day
│   └── vendor/js-yaml.min.js ← js-yaml 4.1.0 (MIT), bundled; nothing loads from the web
├── scripts/
│   ├── folder_tree_builder.R ← leftover helper, unrelated to the app
│   └── cleangit.txt          ← leftover notes
├── HabitTracker.code-workspace
├── project_strucutre_basics.txt  ← leftover notes
├── pyproject.toml            ← Python version only; unused by the app
├── uv.lock
├── .python-version
└── .gitignore                ← still lists token files; harmless, keep
```

`docs/index.html` is no longer generated. It is plain HTML/JS with no framework
and no build; edit it directly.

### habits.yaml

- `habits`: each has `name`, `start` (YYYY-MM-DD), optional `end`, `goal`
  (`daily` or `N per week`), optional `color` and `star`.
- `challenges`: each has `name`, `start`, `end`, optional `habits` list
  (default: all habits).
- `github`: `repo`, `branch` and `file` used for saving check-ins.
- To stop a habit, set an `end` date; never delete it, so history stays.

### checkins.csv

Columns: `date`, one column per habit (`1` = done), `note`. Columns are matched
by habit name; columns for old or removed habits are kept. Oct 1-22, 2026 was
imported from the old Notion data.

### Saving

The page asks for a fine-grained personal access token (Connect GitHub), scoped
to `mayankdate/habit-tracker` only with Contents = Read and write. It is kept in
that browser's storage, which any page on `mayankdate.github.io` can read (all
the owner's Pages sites share the origin); hence the single-repo scope. "Forget
key" removes it. Each tick commits `docs/checkins.csv` about a second later.
Unsaved ticks stay in the browser and retry on next load. On a conflict (two
devices saving at once) the page reloads the file and re-applies its own changes.

### Metrics

Headline per habit is the percent of goal met: each Mon-Sun week asks for the
goal's number of days (prorated for partial weeks); extra days in one week do not
offset another week; today counts only once ticked. Shown per overlapping
challenge (headline = active challenge), for the habit's life ("Since <start>",
hidden when identical to a challenge) and the last 30 days. Streaks: daily habits
in days, weekly habits in weeks met (the current week never breaks a streak),
plus this week's x/N. Challenge cards show day X of Y and the average percent
across their habits.

## Commands

No install or build. To view locally:

```bash
python -m http.server 8000 --directory docs
```

Do not open `docs/index.html` by double-click: the browser blocks reading
`habits.yaml` from a file URL. After pushing, Pages updates in about a minute.

## Change guidelines

- Edit `docs/index.html` directly; keep it dependency-free and build-free.
- Do not add external services, databases or server-side code. Do not load
  scripts from the internet; bundle them in `docs/vendor/`.
- Habit definitions belong in `docs/habits.yaml`, not in the HTML.
- Never delete a habit's column or rows from `checkins.csv`.
- `checkins.csv` is public (the repo is public). Never commit credentials; keep
  token files and `*.env` in `.gitignore`.
- When behavior changes, update `README.md` in the same plain, click-by-click
  style.
