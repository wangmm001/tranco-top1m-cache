# tranco-top1m-cache

Daily snapshots of the [Tranco](https://tranco-list.eu/) anti-manipulation Top 1M list, mirrored by [`wangmm001/feedcache`](https://github.com/wangmm001/feedcache).

## Layout

```
data/
  YYYY-MM-DD_<versionID>.csv.gz     # daily snapshot (version ID in filename for reproducibility)
  YYYY-MM-DD.version.txt            # sidecar: single line = versionID
  current.csv.gz                    # pointer to latest snapshot
  current.version.txt               # pointer to latest versionID
```

The snapshot file is a two-column CSV `rank,domain`. The version ID is Tranco's reproducible list identifier — cite it in papers.

## Consume

```bash
curl -L https://raw.githubusercontent.com/wangmm001/tranco-top1m-cache/main/data/current.csv.gz | zcat | head
VERSION=$(curl -s https://raw.githubusercontent.com/wangmm001/tranco-top1m-cache/main/data/current.version.txt)
echo "Tranco version: $VERSION"
```

## License

Tranco data is licensed under CC-BY 4.0 — attribute:
Pochat, V., van Goethem, T., Tajalizadehkhoob, S., Korczynski, M., Joosen, W. (2019). *Tranco: A Research-Oriented Top Sites Ranking Hardened Against Manipulation*. NDSS.

The README and cron scaffolding in this repo are MIT.

## How it works

Daily GitHub Actions cron (03:45 UTC) calls `wangmm001/feedcache`'s reusable workflow. If the Tranco upstream hasn't published a new list (same `list_id`), no commit is made.
