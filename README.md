# Games Bucket List — Landing Page

Static site for the iOS app *Games Bucket List*. No build step and no third-party requests:
plain HTML plus three stylesheets and four font files, all served from the repo. Opening any
page hits nothing but this origin. Open `index.html` in a browser,
or serve the folder (`python3 -m http.server 4321`) and go to <http://localhost:4321>.

## Pages

| File | Language | Purpose |
| --- | --- | --- |
| `index.html` | English (default) | Landing page |
| `index-de.html` | German | Landing page |
| `privacy.html` / `privacy-de.html` | EN / DE | Privacy policy |
| `support.html` / `support-de.html` | EN / DE | Support and FAQ |
| `impressum.html` / `impressum-de.html` | EN / DE | Legal notice (§ 5 DDG) |
| `assets/landing.css` | — | All shared styling, including the app screen mock-ups |
| `assets/tailwind.css` | — | Generated Tailwind utilities — do not edit by hand |
| `assets/fonts.css` | — | `@font-face` rules for the self-hosted fonts |
| `assets/fonts/` | — | Inter and Nunito as woff2 (SIL OFL 1.1) |

Each page carries its own inline SVG icon sprite and remembers the light/dark choice in
`localStorage`. The two landing pages ship `class="no-js"` on `<html>`, which the head
script strips before the first paint — without it the `.reveal` blocks would stay at
`opacity:0` for anyone whose JavaScript never runs.

Every page also carries a `<div id="nav-top">` immediately before `<nav>`. It is a 1px
sentinel (cancelled out by a negative margin) that an `IntersectionObserver` watches: once
it scrolls out of view the nav gets `.scrolled`, which fades in a soft gradient edge under
the bar. That replaces a permanent 1px divider, so the separation only appears when content
actually runs underneath. Keep the sentinel if you copy the nav to a new page.

## App screens

The three phones in the "A look inside" section are **not screenshots** — they are drawn in
HTML/CSS (`.scr` and the `.m-*` classes in `assets/landing.css`) so they stay sharp at any
size and follow the page theme. Inside a phone, `--u` is one design pixel of a 232 pt wide
screen, so a mock-up scales automatically with whatever phone width it is placed in.

The iPhone frame geometry is the same SVG used on the Kids Bucket List page (Apple's
1470×3000 body ratio, 75/1470 bezel inset).

The screens themselves follow the shipping app: an ambient mesh behind everything, opaque
cards floating on it with no visible frame, a glass capsule tab bar (Home / Library /
Insights / Settings) and the quick-add button riding above it. The German mock-ups use the
app's own translations from `Localizable.xcstrings`, so keep them in step when a string
changes there.

## Following the app

The page is meant to read as the same product as the app, so when the app's design system
moves, these move with it:

- **Ambient mesh** — `.ambient` is `AmbientBackground` flattened into CSS: slowly drifting
  colour behind an opaque base, at the app's own opacities (`.24` light, `.42` dark). Bands
  and cards stay translucent so it carries through the whole page instead of stopping at
  the hero.
- **Type** — Nunito stands in for SF Rounded, which the app uses for titles and numerals.
- **Status tints** — the `--st-*` and `--m-*` tokens mirror `LibraryStatus.tintColor`
  (planned indigo, playing cyan, completed green, paused orange, dropped magenta).

Both stylesheets are linked with a `?v=N` query. Bump the one you changed, otherwise
returning visitors keep the cached version.

## Tailwind

The layout utilities used to come from `<script src="https://cdn.tailwindcss.com">`. That is
not a stylesheet but a program: it read the HTML in the browser and rebuilt the CSS on every
page load, which meant a flash of unstyled page for every visitor, a third-party server as a
single point of failure, and no layout at all without JavaScript. The utilities are now
generated once into `assets/tailwind.css` and committed.

Regenerate it whenever you add a Tailwind class that is not already in the HTML:

```
npx tailwindcss@3 -c tailwind.config.js -i tailwind.in.css -o assets/tailwind.css --minify
```

with `tailwind.config.js` = `{ content: ['*.html'], darkMode: 'class' }` and
`tailwind.in.css` = the three `@tailwind base; components; utilities;` lines. Neither file
lives in the repo — the command is a one-off, not a build step.

`assets/tailwind.css` must stay linked **after** `landing.css`: utilities and the classes in
`landing.css` have the same specificity, so only the order decides, and the CDN appended its
`<style>` at the end too.

## Before going live

- [x] **Beta link** — the primary CTA, the nav button and the closing CTA point at
      TestFlight (`https://testflight.apple.com/join/gEE57wMc`).
- [ ] **App Store link** — once the app leaves beta, search for `TODO` in `index.html`
      and `index-de.html` and swap the TestFlight URL for the store URL (and change the
      labels from "Join the TestFlight beta" / "Beta über TestFlight").
- [ ] **Hosting** — the privacy policy names GitHub Pages as the host. If the site goes
      somewhere else, update section 9 in `privacy.html` / `privacy-de.html`.
- [ ] **Dates** — the privacy policy shows "14 August 2026"; bump it when the content changes.
- [ ] Optional: add `app-icon.png` plus favicon/OG image tags, as on the Kids Bucket List page.

## Fonts

Inter and Nunito used to come from `fonts.googleapis.com`, which handed every visitor's IP
address to Google and put the rendering on someone else's server. They are served from
`assets/fonts/` now.

Google ships these as **variable fonts**: one file per family and subset covers the whole
weight range. That is why `assets/fonts.css` has sixteen `@font-face` blocks but only four
files behind them — the per-weight declarations all point at the same file. It is the exact
structure Google serves, kept deliberately so the rendering does not change.

Only the `latin` and `latin-ext` subsets are included; the site is English and German only,
so Cyrillic, Greek and Vietnamese would be dead weight. Adding a weight means copying the
`@font-face` block from Google and pointing `src` at the family's existing local file.

## Colours

Taken from the app's asset catalog:

| Token | Hex | Asset |
| --- | --- | --- |
| Accent | `#4F46E5` | `BrandIndigo` |
| Spark | `#D946EF` | `SparkMagenta` |
| Discovery | `#22D3EE` | `SparkCyan` |
