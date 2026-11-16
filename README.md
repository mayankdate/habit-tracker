# Habit Tracker

Your habit tracker lives in the browser, here:

https://mayankdate.github.io/habit-tracker/

Tick what you did, and it saves itself to GitHub. There is nothing to build and
no Notion token to look after any more.

## One-time setup (once per browser or device)

The page needs a key to save your ticks.

1. Open https://github.com/settings/personal-access-tokens/new
2. Give it any name, for example `habit tracker`.
3. Under Repository access, choose Only select repositories, then pick
   `mayankdate/habit-tracker`.
4. Under Permissions, open Repository permissions, find Contents and set it to
   Read and write.
5. Click Generate token and copy it.
6. Open the tracker page and click Connect GitHub. Paste the token and click Save key.

The key stays in that browser only. Click Forget key on the page to remove it.
Note: every Pages site under mayankdate.github.io shares one address, so any of
them could read the key. That is why the key is limited to this one repo.

## Every day

1. Open the page.
2. Tick the habits you did today. Add a note if you like.
3. Look at the status line at the bottom. It says when the save is done (about
   a second).

To fill in an earlier day, click the ‹ arrow or the date picker, or click a
square in the calendar. Then tick as usual.

If you are offline or a save fails, your ticks stay in that browser and are
tried again the next time you open the page. If two devices save at the same
moment, the page reloads the file and puts your changes back on top.

## What the numbers mean

- The big number for each habit is the percent of its goal met.
- Each Monday-to-Sunday week asks for the goal's number of days. A short
  first or last week asks for less.
- Extra days in one week do not make up for a missed week.
- Today only counts once you have ticked it.
- You see it for each challenge the habit is part of, for the habit's whole
  life ("Since <start date>"), and for the last 30 days.
- Streaks: daily habits count days, weekly habits count weeks met. The current
  week never breaks a streak. Weekly habits also show this week's progress,
  for example 3/5.
- Each challenge card shows "day X of Y" and the average percent of its habits.

## Changing habits and challenges

Everything is in one settings file, `docs/habits.yaml`. Open it in VS Code.

- A habit has a name, a start date (YYYY-MM-DD), an optional end date, a goal
  (`daily` or `3 per week`, any number), and an optional color and star.
- A challenge has a name, a start, an end, and an optional list of habits
  (if you leave it out, it covers all habits).
- To stop a habit, give it an end date. Do not delete it, so its history stays.

Before editing, pull (Source Control → ... → Pull), because every tick adds a
save on GitHub. After editing: save the file, then commit and push in VS Code. The page
updates in about a minute. Refresh it.

## Your data

Check-ins are in `docs/checkins.csv`, one row per day: the date, a column per
habit (1 means done), and a note. Your old Notion data (Oct 1 to 22, 2026) was
copied in. Columns are matched by habit name, and old columns are kept.

The repo is public, so the check-ins are public too.

## Project layout

```
habit-tracker/
├── docs/
│   ├── index.html            The whole app (written by hand, no build step)
│   ├── habits.yaml           Settings: habits, challenges, where to save
│   ├── checkins.csv          Your check-in data
│   └── vendor/js-yaml.min.js Settings reader (js-yaml 4.1.0, MIT), bundled
├── scripts/
│   ├── folder_tree_builder.R Old helper that prints a folder tree
│   └── cleangit.txt          Old git notes
├── HabitTracker.code-workspace
├── project_strucutre_basics.txt  Old notes
├── pyproject.toml, uv.lock, .python-version  Left over, not used by the app
├── CLAUDE.md                 Notes for AI assistants
└── README.md
```
