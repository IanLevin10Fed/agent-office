# 10 Federal · Agent Office

Interactive top-down ops-floor visualization of Ian’s bots. Built for the **Visual** agent (`ce012f92-2588-4894-8547-5e318c41054a`).

## Open

**Preferred (file:// works offline):**

```text
/workspace/office-viz/index.html
```

Open that path in a browser (or Cursor Simple Browser). Data loads from `activity.embed.js` (no CORS issues).

**HTTP (enables JSON polling):**

```bash
cd /workspace/office-viz && python3 -m http.server 8765
```

Then visit `http://127.0.0.1:8765/`.

Durable copy (same files):

```text
/home/box/agent-data/agents/ce012f92-2588-4894-8547-5e318c41054a/assets/office-viz/
```

## Refresh data

```bash
python3 /workspace/office-viz/update_activity.py
```

Rewrites:

- `activity.json` — snapshot for HTTP fetch
- `activity.embed.js` — `window.OFFICE_ACTIVITY = …` for `file://`

The HTML polls every ~20s (fetch `activity.json` when served over HTTP; otherwise reloads the embed script). After running the updater, wait one poll cycle or reload the page.

## Layout zones

| Zone | Agent | Desk |
|------|--------|------|
| Glass office (center) | Chief of Staff | Central desk |
| Intake (NW) | Property Research | Intake pod |
| Filing (SW-mid) | File Explorer | Filing station |
| Modeling (NE) | Investment Analyst | Modeling desk |
| Diligence (E) | Internal Due Diligence | Diligence room |
| Prospecting (SW) | Off-Market Prospecting | Prospecting desk |
| Outreach (S) | Email Outreach | Outreach desk |
| Observer balcony (N) | Bot Evaluator | Balcony |
| Design studio (SE) | Visual | Studio |
| IC table (center-south) | — | Ian’s meeting spot |

## Status heuristic

- **active** — matches `active-agent.json` or last audit within ~2 minutes  
- **recent** — last audit within ~30 minutes  
- **idle** — older / no audit  

Sources: `profile.json`, `audit.jsonl` tails, `active-agent.json`, light transcript name cues. Summaries are sanitized (no secrets / raw long commands).

## Files

- `index.html` — UI (inline CSS/JS, no CDN)
- `activity.json` / `activity.embed.js` — generated
- `update_activity.py` — scanner
- `assets/` — optional icons

## Limitations

- Opening via `file://` cannot fetch `activity.json` (CORS); embed.js is the primary path. Some browsers may cache embed.js aggressively — hard-reload after an update if the clock looks stale.
- Interaction arcs are inferred from audit/transcript mentions, not a guaranteed message bus.
- “New Bot” (`4fa74d90-…`) is intentionally omitted.
