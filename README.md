# JAF Visuals — Portfolio Site

A single-page photography portfolio. No build tools, no dependencies —
just `index.html` plus an `assets/` folder of images.

## File structure

```
index.html
assets/
  images/
    fashion/       20 images shown in the Fashion & Editorial section
    portraits/     11 images shown in the Portraits section
    maternity/      5 images shown in the Maternity section
    milestones/    11 images shown in the Milestones section
    features/      the 5 large "hero" images (masthead + one per section)
    about-placeholder.jpg
```

## Viewing it locally

Just double-click `index.html` — it opens directly in any browser. No
server or install needed.

## Publishing with GitHub Pages (free hosting)

1. Create a new repo on GitHub and upload everything in this folder
   (`index.html` and the `assets/` folder), keeping the same structure.
2. In the repo, go to **Settings → Pages**.
3. Under "Build and deployment," set **Source** to "Deploy from a branch,"
   pick the `main` branch and `/ (root)` folder, then **Save**.
4. GitHub will give you a live URL (usually
   `https://<username>.github.io/<repo-name>/`) within a minute or two.

## Things to customize before going live

- **Name/brand** — search `index.html` for "JAF Visuals" and replace with
  the real name/logo text (appears in the header, masthead, and footer).
- **Contact info** — replace `hello@jafvisuals.com` and `Columbus, Ohio`
  (appears in the masthead and footer).
- **Bio** — the paragraph in the `<section class="about">` block is
  placeholder text. Swap in the real bio.
- **About photo** — replace `assets/images/about-placeholder.jpg` with an
  actual headshot (keep the same filename, or update the `src` in
  `index.html` if you rename it).
- **Instagram link** — currently a dead `#` link in the About section and
  nowhere else; add the real URL.

## Adding, removing, or swapping photos

Each photo is a plain `<figure>` block inside a `<div class="row cols-2">`
or `cols-3">` in `index.html`, grouped under a `<section class="cat">` per
category. To swap a photo, replace the file in the matching
`assets/images/<category>/` folder and update the `src` in the matching
`<img>` tag (or just overwrite the file with the same name — no HTML
change needed).

To add a new photo to a category, drop the file into that category's
folder and copy an existing `<figure class="ph">...</figure>` block,
pointing its `src` at the new file.

## The "Index" menu

The button in the header opens a full-screen menu (see the
`<div class="index-overlay">` block near the bottom of `index.html`).
Each category name is a link to that section's `id` further up the page
(e.g. `#fashion`) — clicking it closes the menu and scrolls down to that
section. It's all one page; nothing here links to a separate page.
