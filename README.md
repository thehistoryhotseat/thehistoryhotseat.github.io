# History Hot Seat · Command Center

Aryeh's command center: one page that shows what's waiting on Aryeh, how each project is doing, and the latest updates from Tab and Instinct.

- `index.html`: the page. It's plain HTML with inline CSS and JS. No build step, no external files, no trackers, no API keys.
- `data.json`: everything the page shows. **To update the board, edit this file and push.** The page never writes anything.
- The earlier pages (`history-hot-seat.html`, `hangr.html`, `18-consulting.html`) and the PDFs are untouched. The new page links to them.

Tabs are deep-linkable: `#home`, `#hhs`, `#hangr`, `#ai`, `#trade` (a tab's link is `#` plus its project `id`).

## Updating the board

1. Edit `data.json` (the GitHub web editor is fine).
2. Change `"updated"` to today's date, `YYYY-MM-DD`.
3. Check it's valid JSON before you commit. One missing comma breaks the page; it then shows a "Couldn't load the board data" message with the error instead of the board. Any JSON validator works, or run `python3 -m json.tool data.json`.
4. Commit. GitHub Pages usually shows the change within a minute or two.

The "Post update" button doesn't post anything by itself. It opens a pre-filled email to Tab (mytab.5160@mail.tab.bot) and Instinct (aryehbrecher@mail.instinct.com) with the project in the subject. They turn it into a `data.json` change.

To preview locally, run `python3 -m http.server` in this folder and open http://localhost:8000. Opening `index.html` straight from disk won't work, because browsers block a file page from reading `data.json`. The page says so when that happens.

### Add a feed post

Add an object to `"feed"`. Order doesn't matter: the page sorts newest date first, and posts with the same date keep their order in the file, so put the newest at the top.

```json
{ "date": "2026-10-07", "author": "tab", "project": "hangr", "kind": "Shipped",
  "text": "Promo cut v1 is ready for review.", "ask": "Approve the cut" }
```

### Add a "waiting on Aryeh" item

Add an object to that project's `"waiting"` list. Remove it when it's done (and add it to `"shipped"` or `"decisions"` if that's useful).

```json
{ "text": "Approve promo script v2 (44 s)", "by": "Tab", "note": "for the promo" }
```

### Move work along

Each project has `"shipped"`, `"in_progress"`, `"up_next"` and `"decisions"` lists. They all use the same item shape. Move an item from one list to another as it progresses.

## Authorship rules (please keep these)

- **Never post in Aryeh's voice** (`"author": "aryeh"`) unless Aryeh wrote or approved those exact words. If he sends an update by email, quote it as written.
- Instinct's decisions are posted as `"instinct"`, Tab's updates as `"tab"`. Don't turn a decision into a first-person Aryeh post. "Aryeh approved the Ep 1 script" is posted by whoever reported it.
- Only use real information. Where there's nothing real, leave the list empty (the page shows a friendly empty state), or set `"sample": true` so it's labelled SAMPLE.
- **This repo and page are public.** No private numbers, prices, customer data, contact details, logins or secrets.
- Use dates only (`YYYY-MM-DD`). Don't make up clock times.

## data.json schema

Top level:

| field | type | meaning |
|---|---|---|
| `schema_version` | number | `1` |
| `updated` | `YYYY-MM-DD` | shown as "Updated …". It turns amber after 2 days without an update. |
| `updated_by` | string | who made the last edit, e.g. `"Tab"` |
| `owner` | string | name in the greeting (`"Aryeh"`) |
| `projects` | list | one per tab, in tab order (see below) |
| `topics` | list | extra feed tags that aren't tabs, e.g. `{ "id": "cc", "name": "Command center", "short": "Command center", "color": "ink" }` |
| `coming_up` | list | `{ "when": "Oct 15", "date": "2026-10-15", "text": "…", "project": "hhs" }`. `when` is the label shown; `date` is optional. |
| `feed` | list | posts (see below) |

Project:

| field | type | meaning |
|---|---|---|
| `id` | string | short id, also the tab link (`#hangr`). Required. |
| `name` | string | full name. Required. |
| `short`, `icon` | string | short label for chips and the phone tab bar; 2-letter icon |
| `color` | string | `coral`, `teal`, `violet`, `green` or `ink` |
| `tagline` | string | one line under the name on Home |
| `status` | object | `{ "tone": "ok" \| "you" \| "block" \| "idle", "label": "Moving" }`. Tones: ok = green, you = yellow "needs you", block = red, idle = grey. |
| `summary` | string | the one-sentence status at the top of the tab |
| `details` | string | the smaller line under it |
| `next` | string | the "Next up" line on the Home card |
| `stats` | list | `{ "value": "6", "label": "prospects" }`, the big numbers in the header |
| `waiting` | list of items | open items waiting on Aryeh. They feed the yellow box and the tab badges. |
| `shipped`, `in_progress`, `up_next`, `decisions` | lists of items | the board |
| `links` | list | `{ "label": "…", "href": "page.html" }`. Only relative links or `https://` links are allowed. |
| `sample` | boolean | `true` marks the whole tab as SAMPLE |

Item: `{ "text": "…", "by": "Tab", "date": "2026-10-06", "note": "…", "sample": false }`. Only `text` is required.

Post:

| field | meaning |
|---|---|
| `date` | `YYYY-MM-DD` (required) |
| `author` | `tab`, `instinct`, `aryeh` (see the rules above), `site` (carried over from the previous site) or `sample` |
| `project` | a project `id` or a `topics` id |
| `kind` | short label: `Shipped`, `Decision`, `In progress`, `Needs Aryeh`, `Archive`… |
| `text` | the post |
| `ask` | optional. Shows a yellow "Waiting on Aryeh: …" tag. |
| `sample` | optional `true` to label it SAMPLE |

## What's in v1 (Oct 6, 2026)

- History Hot Seat, Hangr and the command center itself: real items from Tab and Instinct, as of Oct 6, 2026.
- 18 AI Consulting: carried over from the previous site (last updated Sep 30), kept general with no contact details.
- Items marked "Previous site" were carried over from the earlier pages (the Franklin pilot, HeyGen access ending Oct 30, the Sep 29 outreach).
- Trading: **SAMPLE** only, until real data is provided.
