# Hackathon Scout

A Claude routine that finds new hackathons relevant to Ryan (freshman at
Archbishop Mitty HS, San Jose CA) and reports them daily.

## How it works

Each run the agent:
1. Reads `state/seen.json` (event URL → metadata of already-reported events).
2. Scans the sources in `profile.json` for hackathons in the next 6 months.
3. Filters by location, eligibility, and timing (see `profile.json`).
4. Writes a dated report to `reports/YYYY-MM-DD.md`.
5. Updates `state/seen.json` (adds new events, prunes past ones).
6. Commits both files to `main`.

## Repo layout

| Path                     | Purpose                                                        |
|--------------------------|----------------------------------------------------------------|
| `state/seen.json`        | Dedupe + deadline tracking. Keyed by event URL. Starts `{}`.   |
| `reports/`               | One markdown report per run, named `YYYY-MM-DD.md`.            |
| `profile.json`           | Scout profile + tunable filters (cities, radius, sources).     |
| `README.md`              | This file.                                                     |

## Tuning

Edit `profile.json` to change the search radius, city list, eligible sources,
or eligibility rules — no need to rewrite the routine prompt. (To have the
agent honor `profile.json` directly, add a step to the routine prompt telling
it to read `profile.json` for filters before scanning.)

## seen.json entry shape

```json
{
  "https://example.com/event": {
    "name": "Event Name",
    "location": "Venue, City — or 'online'",
    "deadline": "YYYY-MM-DD",
    "first_seen": "YYYY-MM-DD",
    "event_date": "YYYY-MM-DD"
  }
}
```

## Routine prompt

The routine prompt is stored at [`ROUTINE_PROMPT.md`](ROUTINE_PROMPT.md) so it
is version-controlled alongside the data.
