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

## One-time setup

### 1. Install Quarto locally (optional, for preview)
Download from https://quarto.org/docs/get-started/

```bash
quarto preview   # live preview in browser
quarto render    # build the site locally → _site/
```

### 2. Push to GitHub
```bash
git init
git add .
git commit -m "initial wiki"
git remote add origin https://github.com/YOUR_ORG/wiki.git
git push -u origin main
```

### 3. Enable GitHub Pages
- Go to repo **Settings → Pages**
- Source: **GitHub Actions**
- Save — the first deploy triggers automatically

Your site will be live at `https://YOUR_ORG.github.io/wiki`

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
