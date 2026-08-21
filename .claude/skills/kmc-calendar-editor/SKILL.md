---
name: kmc-calendar-editor
description: Use when a team member says "update the KMC calendar", "edit the calendar", "change a call on [calendar]", "add/remove a call from [calendar]", "change a calendar link", "add a host to skin in the game", or otherwise asks to modify one of the KMC support calendars in this repo. Finds out which calendar and what change first, then edits the HTML file directly, preserving every fix required for the calendar to actually work correctly on GHL/mobile.
model_tier: workhorse
version: v1.0
---

# KMC Calendar Editor

## What this does

Edits any of the HTML support calendars in this repository — day-column call schedules with a "Skin in the Game" banner, published live via GitHub Pages and embedded into GHL — based on plain-language requests: add/remove/rename a call, change a time, change a link, add/remove a host, etc.

## When to use it

- "Update the KMC calendar"
- "Edit [calendar name]"
- "Remove [call] from [calendar]"
- "Add a call to [calendar]"
- "Change the link for [call]"
- "Add/remove a host on Skin in the Game"
- Any request to modify one of the calendar HTML files in this repo

## How it works

### 1. Find out which calendar

If the request doesn't already name one, list every `.html` file actually present at the repo root (run a directory listing — don't rely on a hardcoded list, since calendars get added or renamed) and ask: **"Which calendar?"**

As of the last time this was written, the calendars living here are:

| File | Calendar |
|---|---|
| `mastery-calendar-updated.html` | Mastery Support Calendar |
| `Elite-blueprint-support-calendar.html` | Elite Support Calendar |
| `mashore-blueprint-cab-support-calendar.html` | CAB Support Calendar |
| `ai-for-agents-blueprint-support-calendar.html` | AI For Agents Blueprint Support Calendar |
| `ai-cab-support-calendar.html` | AI CAB Support Calendar |
| `kmo-calendar.html` | KMO Calendar |
| `ai-mastermind-call-calendar.html` | AI Mastermind Call Calendar |
| `30-day-success-plan.html` | 30 Day Success Plan |

**Always confirm against the actual files in the folder** — this table goes stale the moment someone adds, removes, or renames a calendar.

### 2. Find out what change

If it isn't already clear from the request, ask what's changing: add/remove/rename a call, change a time, change a link, add/remove a host from the "Skin in the Game" popup, etc.

### 3. Make the edit

All calendars share the same structure (Mastery uses a `kmcal-` class prefix; the other seven use `cwk-`; the same rules apply to both):

- Each weekday is a `.cwk-day` / `.kmcal-day` block containing one `.cwk-call` / `.kmcal-call` link per call: a name (`.cwk-n`), a time (`.cwk-t`), and an `href` pointing at the meeting link.
- Cycle through the existing color classes (`-pink`, `-navy`, `-green`, `-purple`, `-orange`, `-blue`) for a new call so colors don't repeat back-to-back in the same column.
- **"Skin in the Game" (or any daily recurring item) is one banner at the top of the page** — never add it back as a repeated card inside each day column; that's exactly the layout we moved away from. Where a calendar has more than one rotating host, that banner opens a popup listing them, with the Join Call button *above* the host list and host names stacked top-to-bottom (never side-by-side). Where there's only one host, the banner is a plain direct link — no popup needed.
- **If no link is given for a new call, do not invent one.** Use a plain `<div>` instead of an `<a>` with the same classes — same visual style, just not clickable — and tell whoever asked that it's unlinked until they supply a real link. Same treatment for anything with no link at all (e.g. a call that says "message your advisor for a link").

### 4. Never break these, no matter how small the edit

These are hard-won fixes from real mobile testing — re-breaking any of them silently reintroduces a bug that already cost real debugging time:

- **Every real link (`href="https://..."`) must keep `target="_blank" rel="noopener noreferrer"`.** These calendars run embedded inside a GHL funnel-inside-product frame. Without `target="_blank"`, a tap navigates inside that nested frame instead of opening a real browser tab, and the phone can't hand off to apps like Zoom. This is the single easiest thing to accidentally undo — double-check it survived after every edit.
- The Google Fonts `<link>` and the page's `<style>` block belong in `<head>`, never in `<body>`.
- Don't touch any day, call, or host that wasn't part of the request — these are live pages other team members and students rely on for their actual schedule.

### 5. After editing

Say plainly that the file is changed **locally only**. Whoever's using Claude needs to commit and push the change themselves in GitHub Desktop (or ask Claude to do it, if they're comfortable with that) before it goes live on the GitHub Pages URL.

Before starting any edit, it's worth reminding them to **pull the latest changes first** if they haven't already this session — someone else on the team may have already changed the same file, and editing an outdated local copy risks losing their change or creating a merge conflict.

## What it produces

The requested edit applied directly to the calendar's HTML file, with every mobile-compatibility fix (target=_blank, head/body structure, single-banner pattern, vertical host list) intact — ready to commit and push.

## Notes

- This repo intentionally holds real, live data (real Zoom links, passcodes, host names). Unlike a shared company skills library, there's no requirement to genericize or scrub this content — it's meant to be edited directly, that's the whole point of this repo existing separately.
- If a request is ambiguous about which call is meant (e.g., two similarly-named calls on the same day), ask rather than guess.
- If someone asks for a change that would apply to multiple calendars at once (e.g. "rename all Office Hours everywhere"), confirm which calendars they mean before touching more than one file.
