# How this profile is built

`README.md` renders on <https://github.com/robertpierson>. Everything in it is either a
static badge or an image generated on a schedule.

## The snake

[`.github/workflows/snake.yml`](../.github/workflows/snake.yml) runs every 12 hours, on
every push to `main`, and on manual dispatch.

1. `Platane/snk@v3` reads the public contribution graph for `robertpierson` and animates a
   snake eating each square.
2. It writes three files into `dist/`: `snake.svg` (light), `snake-dark.svg` (dark), `snake.gif`.
3. `crazy-max/ghaction-github-pages@v4` force-pushes `dist/` to the orphan `output` branch.
4. The README embeds them from `raw.githubusercontent.com/.../output/snake.svg` inside a
   `<picture>` so the dark variant swaps in automatically.

`output` is generated. Never edit it by hand — the next run overwrites it.

## Changing the colours

Palette comes from [robertpierson.github.io](https://github.com/robertpierson/robertpierson.github.io):

| Token | Hex | Used for |
| --- | --- | --- |
| stock | `#F7F4ED` | cream background, empty squares |
| ink | `#0F2419` | body text |
| green | `#1A6B42` | snake, titles, borders |
| green-lift | `#23935B` | icons, accents |
| forest | `#0B1F15` | dark-mode background |
| green-dark | `#6BD79C` | dark-mode snake |

Hexes appear in two places: the `outputs:` block of the workflow (URL-encoded, `%23` for `#`)
and the query strings on the stats cards in `README.md`. Change both.

## Card services

Third-party renderers, so they can rate-limit or go down:

- `github-readme-stats.vercel.app` — stats, top languages, repo pins
- `streak-stats.demolab.com` — streak
- `github-readme-activity-graph.vercel.app` — commit graph
- `github-profile-trophy.vercel.app` — trophies
- `komarev.com/ghpvc` — view counter

A blank image in the README usually means one of these is down, not that the README broke.

## Re-running by hand

```bash
gh workflow run snake.yml --repo robertpierson/robertpierson
gh run watch --repo robertpierson/robertpierson
```
