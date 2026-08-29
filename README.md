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
- **Booking form** — paste your EmailJS keys and Cal.com link into
  `BOOKING_CONFIG` in `index.html` (see "Booking form setup" below).
- **Instagram link** — currently a dead `#` link in the About section and
  nowhere else; add the real URL.

## Booking form setup

The booking section (`#booking`, near the bottom of `index.html`) is a
two-step flow:

1. **The brief** — a custom form (name, phone, email, shoot type, shoot
   description) that sends through **EmailJS**.
2. **Pick a time** — an inline **Calendly** embed showing your real
   availability. Open slots are selectable; anything already booked disappears,
   because Calendly reads your connected calendar.

One submission sends **two** emails via EmailJS:

- a **new booking notification** to you and your friend, and
- a **custom auto-reply** to whoever filled the form.

Calendly owns the calendar and prevents double-booking. Since the two services
share no state, each submission mints a reference code (e.g. `JAF-84V5C`) that
appears in your notification email *and* is pre-filled into the Calendly booking
notes, so a brief can be matched to the slot it belongs to.

### What you need to fill in

Four values in the `BOOKING_CONFIG` block at the top of the last `<script>` in
`index.html`. Nothing else needs editing.

```js
const BOOKING_CONFIG = {
  emailjsPublicKey:      'PASTE-YOUR-EMAILJS-PUBLIC-KEY-HERE',
  emailjsServiceId:      'PASTE-YOUR-EMAILJS-SERVICE-ID-HERE',
  emailjsNotifyTemplate: 'PASTE-NOTIFICATION-TEMPLATE-ID',
  emailjsReplyTemplate:  'PASTE-AUTO-REPLY-TEMPLATE-ID',
  calendlyUrl:           'https://calendly.com/mtanner877/photo-session',
  ...
};
```

**No email addresses go in this file.** Who gets notified is set on the EmailJS
template itself, which keeps your inboxes out of this public repo.

### EmailJS setup (free)

1. Sign up at [emailjs.com](https://www.emailjs.com) and connect the Gmail
   account you want mail to send from (**Email Services** → Add New Service).
   Copy the **Service ID**.
2. **Account → General**: copy the **Public Key**.
3. **Email Templates** → create *two* templates. The free plan allows exactly
   two, which is what this needs.

**Template 1 — the notification (to you and your friend).**
Set **To Email** to both addresses separated by a comma:

```
you@example.com, yourfriend@example.com
```

Subject and body can use any of these variables:
`{{reference}}` `{{name}}` `{{phone}}` `{{email}}` `{{shoot_type}}`
`{{description}}` `{{submitted}}`. Set **Reply To** to `{{email}}` so hitting
reply goes straight to the client. Copy the **Template ID**.

> If a test shows only the first address receiving mail, your provider isn't
> splitting the comma list. Fix it without touching the template: set **To
> Email** to `{{notify_email}}` and list the addresses in `notifyEmails` in
> `BOOKING_CONFIG`. The code then sends one notification per address — at the
> cost of an extra request each, and those addresses become public in this repo.

**Template 2 — the auto-reply (to the client).**
Set **To Email** to `{{email}}`. This is the branded email the client gets the
moment they submit — write it in HTML with your wording and the JAF wordmark.
The same variables are available, so you can echo their brief back to them and
quote `{{reference}}`. Copy the **Template ID**.

You do *not* need EmailJS's "Auto-Reply" tab — the code sends both templates
explicitly, which keeps the two emails independent.

**4. Lock it down.** The public key is visible in the page source (unavoidable
for any no-backend form, and true of every alternative too). In **Account →
Security**, turn on the domain allowlist and add your live domain so the key
can't be reused from anywhere else.

### Calendly setup — already done

The booking page is live at
**https://calendly.com/mtanner877/photo-session** (90 minutes, Columbus area),
and `calendlyUrl` in `index.html` already points at it. Nothing to paste.

Things you may want to change, all in Calendly itself — the site picks them up
automatically, no code edit needed:

- **Your hours.** Availability defaults to 9:00–17:00 every day, Sunday
  included. Set your real shooting hours under **Availability**.
- **Connect your Google Calendar** so anything already in your diary blocks
  those slots off. Until you do, Calendly only knows about Calendly bookings.
- **Session length** is 90 minutes. Change it on the event type if your shoots
  run longer or shorter.
- **Your link name.** The URL says `mtanner877` because that's the account
  username. Changing it to something like `jafvisuals` under **Account →
  Link** makes the booking page look like yours — if you do, update
  `calendlyUrl` in `index.html` to match, or the calendar will 404.

Only change `calendlyUrl` if you rename the event or move bookings to a
different Calendly account.

### Free tier limits

EmailJS free allows **200 requests/month** and 2 templates. Each booking costs
2 requests (notification + auto-reply), so that's **~100 bookings/month**. If
you use the `notifyEmails` fallback above with two addresses, each booking
costs 3 requests instead (~66/month).

Calendly's free plan allows **one** event type and unlimited bookings, which
is exactly what this uses. Calendly's own confirmation email to the client is
free and separate from the EmailJS auto-reply.

### Notes

- Both third-party scripts (EmailJS and Calendly) are **lazy-loaded** — they
  only download when a visitor scrolls near the booking section, so browsing
  the portfolio stays as dependency-free as the rest of the site.
- If Calendly fails to load, the section shows a fallback message pointing at
  your email rather than an empty box.
- Until the values are pasted in, the form and calendar show a "not connected
  yet" message rather than failing silently.
- The auto-reply is sent *after* the notification and is non-fatal: if the
  client's confirmation fails, you still get the enquiry.
- The confirmation screen only appears once the notification has genuinely
  sent, so a failure can never look like a success.
- The form validates client-side and has a hidden `botcheck` honeypot; a
  tripped honeypot shows the normal confirmation but sends nothing.

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
