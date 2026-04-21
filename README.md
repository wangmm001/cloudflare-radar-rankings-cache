# cloudflare-radar-rankings-cache

Snapshots of Cloudflare Radar's DNS domain ranking buckets (1k / 10k / 100k / 1M), mirrored by [`wangmm001/feedcache`](https://github.com/wangmm001/feedcache).

## Layout

```
data/
  YYYY-MM-DD/
    top-1000.csv.gz
    top-10000.csv.gz
    top-100000.csv.gz
    top-1000000.csv.gz
  current/
    top-1000.csv.gz
    top-10000.csv.gz
    top-100000.csv.gz
    top-1000000.csv.gz
```

Each file is the plain-CSV response of Cloudflare Radar's `/radar/datasets/ranking_top_<N>` endpoint — a single column with a `domain` header, one domain per line, **unordered** (just a set of "domains in the top N"). A day's four buckets are atomic: if any fetch fails, the day is skipped and nothing is committed.

## Update cadence

Cloudflare publishes these bucket datasets **weekly** (covering the last 7 days). The cron here runs daily for convenience, but most days the upstream content hasn't changed — deterministic gzip + `git diff --cached --quiet` means no new commit is produced on those days. Expect roughly one commit per week per bucket.

## Consume

```bash
# latest top-1000 bucket
curl -L https://raw.githubusercontent.com/wangmm001/cloudflare-radar-rankings-cache/main/data/current/top-1000.csv.gz | zcat | head
```

## License

The README and scaffolding in this repo are MIT. The mirrored data is subject to [Cloudflare Radar's terms of use](https://www.cloudflare.com/radar/terms/).

## How it works

Daily GitHub Actions cron (04:00 UTC) invokes `wangmm001/feedcache`. Requires repository secret `CLOUDFLARE_RADAR_API_TOKEN` (Radar API token with `Analytics > Read`).
