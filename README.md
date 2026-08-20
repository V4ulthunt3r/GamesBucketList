# Games Bucket List — Landing Page

Static site for the iOS app *Games Bucket List*. No build step: plain HTML, one shared
stylesheet, Tailwind from the CDN for layout utilities. Open `index.html` in a browser,
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

Each page carries its own inline SVG icon sprite and remembers the light/dark choice in
`localStorage`.

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

`assets/landing.css` is linked with a `?v=N` query. Bump it whenever the stylesheet changes,
otherwise returning visitors keep the cached version.

## Before going live

- [ ] **App Store link** — the primary CTA currently points at the `#notify` section.
      Search for `TODO` in `index.html` and `index-de.html` and swap in the store URL
      (and change the button label from "Coming to the App Store" / "Bald im App Store").
- [ ] **Hosting** — the privacy policy names GitHub Pages as the host. If the site goes
      somewhere else, update section 9 in `privacy.html` / `privacy-de.html`.
- [ ] **Dates** — the privacy policy shows "14 August 2026"; bump it when the content changes.
- [ ] Optional: add `app-icon.png` plus favicon/OG image tags, as on the Kids Bucket List page.

## Colours

Taken from the app's asset catalog:

| Token | Hex | Asset |
| --- | --- | --- |
| Accent | `#4F46E5` | `BrandIndigo` |
| Spark | `#D946EF` | `SparkMagenta` |
| Discovery | `#22D3EE` | `SparkCyan` |
