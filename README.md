# cloudflare-radar-rankings-cache

Daily snapshots of Cloudflare Radar's DNS ranking "top buckets" (1k / 10k / 100k / 1M), mirrored by [`wangmm001/feedcache`](https://github.com/wangmm001/feedcache).

## Layout

```
data/
  YYYY-MM-DD/
    top-1000.json.gz
    top-10000.json.gz
    top-100000.json.gz
    top-1000000.json.gz
  current/
    top-1000.json.gz
    top-10000.json.gz
    top-100000.json.gz
    top-1000000.json.gz
```

Each file is the raw JSON response of Cloudflare Radar's `radar/ranking/top` endpoint at the given `limit`. A day's four buckets are atomic: if any fetch fails, the day is skipped and nothing is committed.

## Consume

```bash
# latest 1k bucket
curl -L https://raw.githubusercontent.com/wangmm001/cloudflare-radar-rankings-cache/main/data/current/top-1000.json.gz | zcat | jq '.'
```

## License

The README and scaffolding in this repo are MIT. The mirrored data is subject to [Cloudflare Radar's terms of use](https://www.cloudflare.com/radar/terms/).

## How it works

Daily GitHub Actions cron (04:00 UTC) invokes `wangmm001/feedcache`. Requires repository secret `CLOUDFLARE_RADAR_API_TOKEN` (free-tier Radar API token).
