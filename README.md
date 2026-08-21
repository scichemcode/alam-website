# alam-website

A personal academic site. No build step, no dependencies, no framework — `index.html` is the whole site, and GitHub Pages serves it as-is.

## Files

```
index.html     the site (HTML, CSS, and JS in one file)
404.html       custom not-found page
.nojekyll      tells Pages to skip Jekyll processing
cv.pdf         drop your CV here — the nav already links to it
```

## Getting it live

This repo is named `alam-website`, which makes it a **project site**. It will be served at:

```
https://scichemcode.github.io/alam-website/
```

If you would rather have the clean `https://scichemcode.github.io` address, rename the repo to `scichemcode.github.io` in Settings → General. That is the only difference — the files work either way, and everything in them uses relative paths so nothing breaks.

To publish:

```bash
cd path/to/these/files
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/scichemcode/alam-website.git
git push -u origin main
```

Then **Settings → Pages** → Source: *Deploy from a branch*, branch `main`, folder `/ (root)`. Live in about a minute.

To preview locally before pushing, run `python3 -m http.server` in this folder and open `localhost:8000`.

## What to change first

| What | Where |
|---|---|
| Name, role, intro | `.hero` block near the top of `<body>` |
| Headshot | `.hero-photo` `<img>` — drop a `photo.jpg` in the repo root; the `onerror` handler removes the tag cleanly if it's missing, so the site never shows a broken-image icon |
| News / milestones | `<li class="entry">` items under `#news`, newest first |
| Research threads | the four `<li>` items under `#research` |
| Publications | `<li class="entry">` items under `#publications` (see below) |
| Skills | `.skill-group` blocks under `#skills` |
| Education, talks, teaching | `<li class="entry">` items in each section |
| Email, GitHub, LinkedIn | the `.contact-list` under `#contact` |
| Colors | the five hex values in `:root` at the top of the `<style>` block |
| Fonts | the Google Fonts `<link>` plus `--display`, `--body`, `--mono` |

## Adding your photo

Export a square-ish headshot as `photo.jpg` and drop it in the repo root — the `<img class="hero-photo">` tag already points at it. Until the file exists, the `onerror` handler removes the broken image automatically, so the hero degrades gracefully with no photo.

## Adding publications

The `#publications` section currently shows an empty-state paragraph plus an HTML comment with a ready-to-uncomment template. When you have a paper, delete the placeholder paragraph, uncomment the template, and duplicate the `<li class="entry">` per publication — the year column and title/venue/links structure already work for citations:

```html
<li class="entry">
  <span class="entry-when">2026</span>
  <div>
    <p class="entry-title">Paper title</p>
    <p class="entry-where">Authors &middot; <em>Journal</em>, volume, pages</p>
    <p class="entry-links"><a href="#">DOI</a><a href="#">arXiv</a><a href="#">Code</a></p>
  </div>
</li>
```

## Adding a skill

Each box under `#skills` is a `.skill-group` with a title and a `.tags` list — add an `<li>` to the relevant group, or copy a `.skill-group` block for a new category.

## Adding news

Copy an `<li class="entry">` under `#news` and put it at the top — the list reads newest first, same markup as talks/education.

## Your CV

Drop your CV in the repo root as `cv.pdf`. The nav and the contact list both already link to it. Re-export and re-push whenever it changes — that link is the one people actually click.

## Notes on the design

The background carries a faint square lattice. Section headings sit in a left margin rail like labels in a notebook. The square in the hero is a live periodic cell — twelve particles drift, wrap around the boundaries, and cast ghost images on the opposite face when they get close to one, which is how a PBC cell is conventionally drawn. It respects `prefers-reduced-motion` and renders a single static frame for anyone who has that set.

If you want it gone, delete the `.cell` div and the `<script>` block at the bottom.

## When one file stops being enough

Once you add a second page, pull the CSS out so the two pages share it:

1. Move everything between `<style>` and `</style>` into `assets/style.css`.
2. Replace the `<style>` block with `<link rel="stylesheet" href="assets/style.css">`.
3. Same idea for the `<script>` → `assets/site.js`, loaded with `<script src="assets/site.js" defer></script>`.

Keep the paths relative (`assets/…`, no leading slash). On a project site a leading slash points at `scichemcode.github.io/assets/…`, which does not exist — this is the single most common way a project site breaks.

## Visitor count

The footer shows a live visitor count from [visitor-badge.laobi.icu](https://visitor-badge.laobi.icu/), a free third-party badge service — there's no backend on this site to count visits itself. It's a single `<img>` tag keyed by `page_id=scichemcode.alam-website`; each page load pings that service, which returns an SVG with the running count. Colors are set via `left_color`/`right_color` query params to match the site palette. If you rename the repo or fork this site, change `page_id` to something unique to you, or the count will be shared with whoever else used that id first.

## Custom domain

Add a file named `CNAME` containing just your domain:

```
kritialam.com
```

Then point a `CNAME` DNS record at `scichemcode.github.io`, or four `A` records at GitHub's Pages IPs if you want the apex domain. Tick **Enforce HTTPS** in Settings → Pages once the certificate is issued — it takes a few minutes.

## Why `.nojekyll`

Pages runs everything through Jekyll by default, which ignores files and folders starting with an underscore. `.nojekyll` turns that off. It costs nothing and saves a confusing afternoon later.
