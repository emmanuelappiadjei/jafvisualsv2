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
- **Booking form** — paste your Web3Forms key and Cal.com link into
  `BOOKING_CONFIG` in `index.html` (see "Booking form setup" below).
- **Instagram link** — currently a dead `#` link in the About section and
  nowhere else; add the real URL.

## Booking form setup

The booking section (`#booking`, near the bottom of `index.html`) is a
two-step flow:

1. **The brief** — a custom form (name, phone, email, shoot type, shoot
   description) that POSTs to **Web3Forms**, which emails the enquiry to you.
2. **Pick a time** — an inline **Cal.com** embed showing your real
   availability. Open slots are selectable; anything already booked is greyed
   out, because Cal.com reads your connected calendar.

They're deliberately two separate services: Web3Forms handles "tell us about
the shoot," Cal.com owns the calendar and prevents double-booking. To tie the
two together, each submission generates a reference code (e.g. `JAF-84V5C`)
that appears in your notification email *and* is pre-filled into the Cal.com
booking notes.

### What you need to fill in

Both values live in one `BOOKING_CONFIG` block at the top of the last
`<script>` in `index.html`. Nothing else needs editing.

```js
const BOOKING_CONFIG = {
  web3formsAccessKey: 'PASTE-YOUR-WEB3FORMS-ACCESS-KEY-HERE',
  calLink: 'PASTE-YOUR-CAL-COM-LINK-HERE',
  ...
};
```

**1. Web3Forms access key** — go to [web3forms.com](https://web3forms.com),
enter your email, and copy the key it sends you. Paste it as
`web3formsAccessKey`. Free plan: 250 submissions/month.

**2. Getting BOTH of you emailed** — in the Web3Forms dashboard, add your
friend's address under **Linked Emails**. The free plan carries 3 addresses
(1 primary + 2 linked), so you're both notified on every submission at no
cost. Don't use the `ccemail` field for this — that one is PRO-only.

**3. Cal.com link** — create the event type at
[cal.com](https://cal.com) and connect your Google Calendar so booked time
is blocked out automatically. Then take the URL *without* the domain: a page
at `https://cal.com/jafvisuals/photo-session` becomes
`'jafvisuals/photo-session'`.

Set your working hours, session length, and buffer time on the Cal.com event
— that's what drives the visible open/booked blocks. Nothing about
availability is stored in this repo.

### Emails: who gets what

| Trigger | Who it goes to | Cost |
|---|---|---|
| Form submitted | You + your friend (Web3Forms linked emails) | Free |
| Time slot booked | The client, plus you — Cal.com confirmation + calendar invite | Free |
| Branded auto-reply the instant the form is sent | The client | **Web3Forms PRO** |

The client already gets a confirmation email from Cal.com when they book a
slot, so the free setup covers them. If you specifically want your own
branded auto-reply to fire on *form submit* (before they've picked a time),
that's the Web3Forms **autoresponder**, which needs their PRO plan
(~$12/month billed yearly). Nothing in the code changes — you enable it in
the Web3Forms dashboard, and it keys off the `email` field the form already
sends.

### Notes

- The Cal.com script is **lazy-loaded** — it only downloads when a visitor
  scrolls near the booking section, so browsing the portfolio stays as
  dependency-free as the rest of the site.
- Until you paste the two values in, the form and calendar show a polite
  "not connected yet" message instead of failing silently.
- The form validates client-side and has a hidden `botcheck` honeypot field
  for spam.
- Reply-to on your notification email is set to the client's address, so
  hitting reply goes straight to them.

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
