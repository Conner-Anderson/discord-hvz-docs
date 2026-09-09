# discord-hvz-docs

Documentation for the Discord bot, discord-hvz, built with MkDocs Material and versioned with Mike.

## Work on the docs

Install [uv](https://docs.astral.sh/uv/getting-started/installation/), then run these commands from this folder:

```powershell
uv sync --locked
uv run mkdocs serve
```

Edit the Markdown pages in `docs/`. The preview reloads as you save changes.

To check the site before reviewing or publishing changes:

```powershell
uv run mkdocs build --strict
```

This writes a local build to `site/`. For versioned previews and publishing, see [HOW TO USE MKDOCS WITH MIKE.txt](<HOW TO USE MKDOCS WITH MIKE.txt>).
