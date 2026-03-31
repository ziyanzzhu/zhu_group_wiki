# Group Wiki — Complete Setup & Contribution Guide

This tutorial covers everything your group needs: setting up the wiki, understanding GitHub accounts and branches, and the day-to-day editing workflow using VS Code.

---

## Part 1 — GitHub accounts: do you need a group account?

**Short answer: No.** Everyone uses their own personal GitHub account.

Here is how it works:

- One person creates the repository (the repo owner — probably you)
- The owner invites other members as **collaborators** via Settings → Collaborators
- Each collaborator uses their own GitHub account to push changes
- GitHub tracks who made which change by their account

A shared "group" account is only useful if you want the repo URL to show an organization name (e.g. `github.com/mylab/wiki` instead of `github.com/yourname/wiki`). To do this, create a free **GitHub Organization** at github.com/organizations/new and transfer the repo there. This is optional — you can always do it later.

**Summary:**

| Scenario | Recommendation |
|---|---|
| Small group, informal | Personal account, add collaborators |
| Lab or research group | GitHub Organization (free) |
| Need private repo | GitHub Free supports private repos |

---

## Part 2 — What is a branch?

Think of the repo as a shared Google Doc. The `main` branch is the live published version of the wiki. A **branch** is your own private copy where you can make edits without affecting the live site.

```
main (live site)
 │
 ├── alice/update-methods    ← Alice editing methods.qmd
 └── bob/add-results         ← Bob adding a new page
```

When Alice is done, she opens a **Pull Request** — a request to merge her branch into `main`. Once merged, GitHub Actions rebuilds the site and the changes go live.

**Why branches matter:**

- Two people editing `main` at the same time causes a merge conflict
- Branches let everyone work in parallel safely
- You can review changes before they go live

For a small group just starting out, you can skip branches and push directly to `main`. Add the branch workflow once 3 or more people are contributing regularly.

---

## Part 3 — One-time setup (do this once per computer)

### Step 1 — Install the tools

Install these in order:

**Git**
```powershell
winget install --id Git.Git -e --source winget
```
Restart PowerShell after this finishes.

**Quarto**
Download the installer from https://quarto.org/docs/get-started/ and run it.

**VS Code**
Download from https://code.visualstudio.com and install.

**Python** (needed to render notebooks)
```powershell
winget install Python.Python.3.11
```

**Jupyter**
```powershell
pip install jupyter numpy matplotlib pandas
```

### Step 2 — Install VS Code extensions

Open VS Code, press `Ctrl+Shift+X` to open Extensions, and search for and install:

- **Quarto** — live preview, syntax highlighting, LaTeX rendering
- **Python** — Jupyter notebook support
- **GitHub Pull Requests** — manage branches and PRs without leaving VS Code (optional but useful)

### Step 3 — Configure Git with your identity

Open a terminal in VS Code (`Ctrl+`` `) and run:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

This is how your name appears next to each commit in the repo history.

### Step 4 — Clone the repo

```bash
git clone https://github.com/YOUR_ORG/wiki.git
cd wiki
```

This downloads the repo to your computer. You only do this once.

---

## Part 4 — Setting up the repo (owner only, also once)

If you are the person creating the wiki for the first time:

```bash
# In the quarto-wiki folder you downloaded earlier
cd quarto-wiki
git init
git add .
git commit -m "initial wiki setup"
git branch -M main
git remote add origin https://github.com/YOUR_ORG/wiki.git
git push -u origin main
```

Then enable GitHub Pages:

1. Go to your repo on GitHub
2. Click **Settings → Pages**
3. Under Source, select **GitHub Actions**
4. Save

The first deployment starts automatically. Your site will be live at `https://YOUR_ORG.github.io/wiki` within a couple of minutes.

To invite collaborators:

1. Go to **Settings → Collaborators**
2. Click **Add people**
3. Enter each person's GitHub username

---

## Part 5 — Day-to-day editing workflow

### Open the project in VS Code

```bash
cd wiki
code .
```

Or open VS Code and use **File → Open Folder** to select the `wiki` folder.

### Start the live preview

In the VS Code terminal:

```bash
quarto preview
```

A browser window opens at `http://localhost:4444`. Every time you save a file, the browser refreshes automatically. This is your local preview — it does not affect the live site until you push.

### Edit a page

Open any `.qmd` file in VS Code and start editing. The preview updates on save (`Ctrl+S`).

Example — adding a new section to `docs/methods.qmd`:

```markdown
## New experiment

We tested three conditions at $t = 0, 24, 48$ hours.

$$
\Delta C = C_t - C_0
$$
```

Save the file and the preview updates instantly.

### Add a new page

1. Create a new file, e.g. `docs/results.qmd`
2. Add a YAML header at the top:

```yaml
---
title: "Results"
date: last-modified
author: "Your Name"
---

Your content here.
```

3. Add it to the navbar in `_quarto.yml`:

```yaml
navbar:
  left:
    - href: docs/results.qmd
      text: Results
```

### Commit and push your changes

When you are happy with your edits:

```bash
# See what files you changed
git status

# Stage all changes
git add .

# Commit with a short description
git commit -m "add results section to methods page"

# Push to GitHub
git push
```

GitHub Actions picks up the push, rebuilds the site, and deploys it. The live site updates in about 1–2 minutes.

---

## Part 6 — Branch workflow (recommended for 3+ contributors)

### Create a branch before editing

```bash
# Make sure you have the latest version of main first
git pull

# Create and switch to a new branch
git checkout -b your-name/describe-your-change
```

Example:
```bash
git checkout -b alice/update-methods
```

Edit your files, then commit as usual:

```bash
git add .
git commit -m "update sample sizes in methods"
git push -u origin alice/update-methods
```

### Open a Pull Request

1. Go to the repo on GitHub — you will see a banner saying your branch has recent pushes
2. Click **Compare & pull request**
3. Write a short description of what you changed
4. Click **Create pull request**
5. The repo owner (or anyone with access) reviews it and clicks **Merge**

Once merged, the branch can be deleted and `main` rebuilds and deploys automatically.

### Switch back to main after merging

```bash
git checkout main
git pull
```

---

## Part 7 — Notion to wiki workflow

For group members who prefer drafting in Notion:

1. Write your content in Notion as usual
2. When ready, click **Export → Markdown & CSV**
3. Rename the exported file to something like `your-page.qmd`
4. Open the file and add a YAML header at the top:

```yaml
---
title: "Your Page Title"
date: last-modified
author: "Your Name"
---
```

5. Clean up any Notion-specific formatting if needed (tables and callouts usually export cleanly; images need to be re-uploaded manually)
6. Place the file in the `docs/` folder
7. Commit and push as described in Part 5

---

## Part 8 — Quick reference

### Commands you will use every day

| What | Command |
|---|---|
| Get latest changes from GitHub | `git pull` |
| See what you changed | `git status` |
| Stage all changes | `git add .` |
| Commit staged changes | `git commit -m "your message"` |
| Push to GitHub | `git push` |
| Start live preview | `quarto preview` |
| Build site locally | `quarto render` |

### Branch commands

| What | Command |
|---|---|
| Create and switch to new branch | `git checkout -b name/description` |
| Switch to an existing branch | `git checkout branch-name` |
| Switch back to main | `git checkout main` |
| See all branches | `git branch` |
| Push a new branch | `git push -u origin branch-name` |

### If something goes wrong

**You pushed something you didn't mean to:**
```bash
git revert HEAD
git push
```
This creates a new commit that undoes the last one — safer than deleting history.

**You have a merge conflict:**
VS Code highlights conflicting lines in red/green. Edit the file to keep the version you want, remove the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`), then:
```bash
git add .
git commit -m "resolve merge conflict"
git push
```

**You want to discard local changes and start fresh:**
```bash
git checkout -- .
```

---

## Part 9 — File structure reference

```
wiki/
├── _quarto.yml              ← site config, navbar, theme
├── index.qmd                ← home page
├── docs/
│   ├── methods.qmd          ← add pages here
│   ├── references.qmd
│   └── results.qmd
├── notebooks/
│   └── analysis.ipynb       ← Jupyter notebooks go here
└── .github/
    └── workflows/
        └── publish.yml      ← do not edit this unless adding Python packages
```

To add a Python package to the build (so notebooks that import it render correctly on GitHub):

Open `.github/workflows/publish.yml` and find this line:

```yaml
run: pip install jupyter numpy matplotlib pandas
```

Add your package to the list:

```yaml
run: pip install jupyter numpy matplotlib pandas scipy scikit-learn
```

---

*This guide can live in your wiki at `docs/contributing.qmd` so new group members always have it handy.*
