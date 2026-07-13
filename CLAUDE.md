# LifeOS — Prince Tony's Personal Life Operating System

## What this project is
A personal life dashboard website built as a single `index.html` file, deployed on GitHub Pages. It is a private tool for daily discipline tracking, weekly life scoring, and a dashboard overview of all life domains. It is not a public product — it is a personal operating system.

## Tech stack
- **Frontend:** Vanilla HTML, CSS, JavaScript — single file, no framework, no build step
- **Database:** Supabase (PostgreSQL) for cloud sync across devices
- **Deployment:** GitHub Pages via the LifeOS repo
- **No Node.js, no npm, no bundler** — keep it simple

## Credentials
```
SUPABASE_URL  = 'https://vjumuvdzrerxoxpjcfwl.supabase.co'
SUPABASE_KEY  = 'sb_publishable_ZDZTWno-Wg4ip3_PqKMl0Q_7DjwWfDQ'
```
These live in the `<script>` block near the bottom of `index.html`. Find them by searching for `SUPABASE_URL`.

## GitHub
- **Repo:** https://github.com/Prince-Tony-7/LifeOS
- **Branch:** main
- **Live URL:** https://prince-tony-7.github.io/LifeOS
- **Deploy:** push to main → GitHub Pages auto-deploys within 2 minutes

## Supabase database tables
Two tables exist in the project's Supabase instance:

```sql
discipline_log (
  date text primary key,       -- e.g. "2026-07-13"
  entry jsonb not null,        -- full daily entry object
  updated_at timestamptz default now()
)

score_log (
  week text primary key,       -- e.g. "W28 2026"
  scores jsonb not null,       -- domain scores object
  updated_at timestamptz default now()
)
```
Row Level Security is disabled on both tables (personal tool, no auth needed).

## File structure
```
LifeOS/
├── index.html    ← entire app lives here
└── CLAUDE.md     ← this file
```

## What the site contains
Three sections accessible via sidebar navigation:

### 1. Dashboard
- Life domain cards for 8 areas: Faith, Ministry, Finance, Career, GridNodes, Health, Relationships, Family
- Priority stack showing ranked life priorities
- Ministry roles tracker (Jesus Youth Music Coordinator Ireland, Nightfever Coordinator Dublin, Emmaus, Global2033)
- Career tracks tracker (Validation/CQV primary, Automation secondary)
- Core blocker reminder about discipline consistency

### 2. Daily disciplines
19 daily habits across 5 categories:
- **Spiritual:** Mass, Rosary, Adoration, DMC, Bible reading, Personal prayer, JY/Ministry activity
- **Physical:** Workout 30min, Breakfast, Lunch, Dinner
- **Social:** Replied to everyone, Called/talked with (name)
- **Mental:** Wake up time, Sleep time, Rest notes
- **Intellectual:** Book reading 30min, Work on project
Each day's entry saves to `discipline_log` in Supabase keyed by date.
Shows daily completion percentage and current streak.

### 3. Weekly scores
8 domain sliders (1–10) for Faith, Ministry, Finance, Career, GridNodes, Health, Relationships, Family.
Saves to `score_log` in Supabase keyed by week label (e.g. "W28 2026").
Trends tab shows line chart of scores over time with week-on-week delta.

## Sync behaviour
- On load: pulls all history from Supabase, merges with localStorage cache
- On save: writes to both Supabase and localStorage
- If Supabase is unreachable: falls back to localStorage silently
- Sync status shown in sidebar: Synced ✓ / Saving… / Local only

## Design
- Dark theme: background #0c0c10, surface #13131a, accent purple #8b82f0
- Font: Inter (body) + Crimson Pro (display headings and large numbers)
- Mobile responsive with hamburger nav below 768px
- No gradients, no shadows — flat dark surfaces

## Owner context
Prince Tony is a Manufacturing Process Engineer in Dublin working toward a career move into Validation/CQV Engineering. He runs an AI automation side business called GridNodes using n8n. He is Catholic, involved in ministry leadership across Ireland, and is using this OS to bring discipline and purpose to all areas of his life.

## Development principles
- Keep everything in a single HTML file unless there is a compelling reason to split
- Never introduce a framework or build system unless explicitly asked
- Always test that data saves and loads from Supabase correctly after any change
- Keep the dark design consistent — do not introduce light colours or gradients
- After any edit, push to the main branch and confirm the GitHub Pages URL is live

## Common tasks
- **Add a new discipline field:** add it to the CATS array in the script block
- **Add a new life domain card:** add a `<div class="domain-card">` block in the dashboard HTML
- **Change Supabase credentials:** find the two const lines near the bottom of the script block
- **Add a new page/section:** add a nav-link button and a `<div class="page">` block, wire up in showPage()
