# Group Wiki

A collaborative research wiki built with [Quarto](https://quarto.org) and hosted on GitHub Pages.

## Structure

```
quarto-wiki/
├── _quarto.yml              ← site config and navigation
├── index.qmd                ← home page
├── docs/
│   ├── methods.qmd          ← methods and protocols
│   └── references.qmd       ← papers and links
├── notebooks/
│   └── analysis.qmd         ← Jupyter notebook pages
└── .github/
    └── workflows/
        └── publish.yml      ← auto-build on push
```

## Adding content

### Option A — write directly
Create or edit `.qmd` files in `docs/` or `notebooks/`, then push.

### Option B — from Notion
1. Export a Notion page as **Markdown & CSV**
2. Rename the file to `.qmd`
3. Add a YAML front matter block at the top:
   ```yaml
   ---
   title: "Your Page Title"
   date: last-modified
   ---
   ```
4. Commit to `docs/` and push

### Option C — Jupyter notebooks
Place `.ipynb` files in `notebooks/` and add them to `_quarto.yml` navbar.

## Adding Python packages
Edit the `pip install` line in `.github/workflows/publish.yml`.
