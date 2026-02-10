# Steady Step System (MkDocs + Material)

A premium “knowledge atlas” for the Steady Step System: a lifestyle lab built on small, measurable experiments (Micro‑Missions, Real Youth Problems, weekly YouTube experiments).

## What’s inside

- `mkdocs.yml` — site configuration (theme, nav, plugins)
- `docs/` — all site pages (Markdown)
- `docs/assets/stylesheets/extra.css` — custom styling (sports‑tech lab look)

## Quick start (local preview)

### 1) Install
```bash
pip install mkdocs-material
### 1) Install
```bash
pip install mkdocs-material

2) Add the next two commands under it (so the README is complete for anyone who clones the repo): `mkdocs serve` and `mkdocs build`.   
3) Save/Commit the `README.md` changes in GitHub.[1]

## Step 2 — Add GitHub Actions deploy (most important)
Create a new file in your repo with this exact path: `.github/workflows/ci.yml` (GitHub will create the folders automatically when you type the slashes).[1]

Paste this exact content (this is the standard Material for MkDocs GitHub Actions workflow):[1]

```yaml
name: ci
on:
  push:
    branches:
      - master
      - main
permissions:
  contents: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure Git Credentials
        run: |
          git config user.name github-actions[bot]
          git config user.email 41898282+github-actions[bot]@users.noreply.github.com
      - uses: actions/setup-python@v5
        with:
          python-version: 3.x
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV
      - uses: actions/cache@v4
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: ~/.cache
          restore-keys: |
            mkdocs-material-
      - run: pip install mkdocs-material
      - run: mkdocs gh-deploy --force
