# lantern-feeds

Published data feeds for [Life Lantern](https://life-lantern.fly.dev), written by the
Hermes fleet's `lantern_events` lane. **Public event data only** — no family data, no
origins, no telemetry, ever.

## Contract

`events/<region>.json`, schema `lantern-events/v1` (additive; breaking changes bump the
schema string):

```json
{ "schema": "lantern-events/v1",
  "region": "<region-key>",
  "generated_at": "<ISO-8601 UTC>",
  "events": [
    { "name": "...", "date": "YYYY-MM-DD", "venue": "...", "town": "...",
      "url": "https://...", "source": "<source name>" }
  ] }
```

- Every event carries the URL of the source that listed it.
- Past-dated events are dropped at publish.
- Heuristically-extracted events carry `"extraction": "heuristic"`.
- Updates land ~2x/week (Mon + Fri mornings, US Pacific). Commit messages carry
  per-region counts — the history is the changelog.

## Consumer

The Life Lantern server fetches these files read-only over plain HTTPS
(raw.githubusercontent.com). No secrets, no auth, no cookies.

- `northshore` currently has no consumer — Life Lantern's v1 keeps ParentMap for
  that region instead — but publishing stays on to keep the option of swapping
  later open.
