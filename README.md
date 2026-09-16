# harrisonwsmith.github.io

Personal research website. Built with [Quarto](https://quarto.org), hosted on GitHub Pages.

---

## One-time setup

### 1. Install Quarto

Download the installer for your OS from <https://quarto.org/docs/get-started/>.
Verify:

``` bash
quarto --version
```

An editor with a Quarto extension makes this much nicer. Either works:

- **VS Code** plus the Quarto extension
- **RStudio** (Quarto support is built in)

### 2. Create the repository

The repository name must match your username exactly for a user site:

``` bash
gh repo create harrisonwsmith.github.io --public --source=. --remote=origin
```

Or create it through the GitHub web UI, then:

``` bash
git init
git remote add origin https://github.com/harrisonwsmith/harrisonwsmith.github.io.git
```

### 3. First commit and push

``` bash
git add .
git commit -m "Initial site scaffold"
git branch -M main
git push -u origin main
```

### 4. Turn on Pages

The included GitHub Action renders the site and pushes it to a `gh-pages`
branch on every push to `main`. The branch does not exist until the Action
runs once, so:

1. Push to `main` and wait for the Action to finish (Actions tab)
2. Go to **Settings > Pages**
3. Set **Source** to *Deploy from a branch*, branch `gh-pages`, folder `/ (root)`
4. Wait a minute, then open <https://harrisonwsmith.github.io>

---

## Daily workflow

``` bash
quarto preview        # live reload at localhost:4200, leave it running while you edit
quarto render         # one-off full build into _site/
```

To publish: commit and push to `main`. The Action does the rest.

``` bash
git add .
git commit -m "Update research page"
git push
```

If you would rather skip CI and publish straight from your machine, delete
`.github/workflows/publish.yml` and use:

``` bash
quarto publish gh-pages
```

---

## What to edit

| File | Contains |
|---|---|
| `index.qmd` | Home page: bio, interests, education |
| `research.qmd` | Three research thrusts, one figure each |
| `publications.qmd` | Renders automatically from `publications.bib` |
| `publications.bib` | **Add papers here.** Nothing else to edit. |
| `cv.qmd` | Web CV |
| `_quarto.yml` | Site title, navigation, theme, footer |
| `assets/styles.scss` | All colors, fonts, spacing |
| `images/` | Headshot and research figures |
| `assets/cv.pdf` | CV download, currently a placeholder |
| `drafts/tools.qmd` | Not published. Excluded from render. |

Placeholder text is wrapped in a yellow `.todo` box. Search the project for
`todo` or for `[` to find everything still needing your words. Delete each box
as you fill it in.

---

## Checklist before going live

- [ ] Replace `images/headshot.jpg`
- [ ] Replace the three `images/research-*.jpg` figures
- [ ] Write the bio on `index.qmd`
- [ ] Write the three research paragraphs on `research.qmd`
- [ ] Paste the full Scholar BibTeX export into `publications.bib`
- [ ] Set your preferred contact email in `index.qmd` (currently `REPLACE-WITH-PREFERRED-EMAIL`)
- [ ] Set your ORCID URL in `index.qmd` (currently `REPLACE-WITH-ORCID`)
- [ ] Fill dates and empty sections in `cv.qmd`
- [ ] Replace `assets/cv.pdf` with the real export
- [ ] Delete every `.todo` box
- [ ] Check the site on a phone

---

## Changing the look

Everything visual lives in `assets/styles.scss`. The palette is defined at the
top of the `scss:defaults` block:

``` scss
$ink:        #16191c;   // headings
$body-color: #2e3338;   // body text
$muted:      #6d757b;   // secondary text
$accent:     #2f5d50;   // pine green, links and highlights
$paper:      #fdfcfa;   // page background
$rule:       #e5e2dc;   // hairlines
```

Change `$accent` and the whole site shifts. Fonts are imported on the first
line of the same file.

---

## Adding a custom domain later

1. Buy the domain (Cloudflare and Namecheap are both fine, roughly $12 to $20/year)
2. At your registrar, add four `A` records for the apex pointing to
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`,
   and a `CNAME` for `www` pointing to `harrisonwsmith.github.io`
3. In **Settings > Pages**, enter the domain under *Custom domain*
4. Tick **Enforce HTTPS** once the certificate provisions
5. Update `site-url` in `_quarto.yml`

Old `harrisonwsmith.github.io` links keep redirecting, so nothing breaks.

---

## Adding pages that run code

Quarto renders Python and R inside `.qmd` files, so an analysis can be a page.

```` markdown
```{python}
#| echo: false
#| fig-cap: "County-level SHAP values for maximum temperature."
import matplotlib.pyplot as plt
...
```
````

If you do this, add a `requirements.txt` and uncomment the Python steps in
`.github/workflows/publish.yml`. Keep `execute: freeze: auto` in `_quarto.yml`
(already set) so pages only re-run when their source changes.
