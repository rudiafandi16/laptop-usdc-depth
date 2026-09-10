# LAPTOP-USDC pool depth — auto-refreshed to Dune

Reproduces Dune query 7685210 (hourly / N-minute pool depth) straight from a Base RPC and
uploads the CSV to Dune every day via GitHub Actions.

- `main.go` — the pipeline (see `CLAUDE.md` for flags and semantics)
- `upload_dune.py` — pushes a CSV to Dune's table upload API (replaces the table)
- `.github/workflows/refresh.yml` — daily 01:30 UTC run for LAPTOP-USDC (0x99cf3e8bfb02c300312c53aac5d0b082e3d5975c, USDC = token0) at 15-minute buckets

## Setup

1. Push this folder to a GitHub repo.
2. Repo → Settings → Secrets and variables → Actions → **New repository secret**:
   `DUNE_API_KEY` = your Dune API key (Dune → Settings → API).
3. Actions tab → "Refresh LAPTOP-USDC depth" → **Run workflow** to test.
4. Query it on Dune:

```sql
SELECT date_parse(hour, '%Y-%m-%dT%H:%i') AS ts, *
FROM dune.<your_handle>.dataset_laptop_usdc_depth_15m
ORDER BY 1
```

To track another pool, copy the job in `refresh.yml` and change `POOL`, `QUOTE`, `START`,
`OUT`, `DUNE_TABLE`.
