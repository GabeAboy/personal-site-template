# Personal Site Template

A zero-build, single-page personal site you customize by editing one JSON file.
Drop in your name, bio, socials, and a few project screenshots, push, done.
No framework, no `npm install`, no toolchain.

## Quickstart

1. Click **Use this template** at the top of the repo (or fork it).
2. Edit `config.json`: name, bio, location, socials, companies, projects, theme.
3. Edit the `<title>`, description, canonical URL, and Open Graph tags at the
   top of `index.html`. Link-preview crawlers don't run JavaScript, so these
   can't come from `config.json`.
4. Replace `assets/portrait.svg` with your own photo (and update
   `profile.avatar` if you change the filename).
5. Add a 1200×630 `assets/og-image.png` for link previews.
6. Drop project screenshots into `assets/projects/<slug>/` and point the
   `slides` entries at them. The sample SVGs are placeholders; delete them.
7. Create a Render **Static Site** pointed at the repo (no build command,
   publish path `.`), or use the included `render.yaml` via **New → Blueprint**.
   Any static host works (GitHub Pages, Netlify, Cloudflare Pages).
8. Push to `main`.

Preview locally first:

```sh
python3 -m http.server 8000
# open http://localhost:8000
```

## How it works

- `index.html` is a static shell with three mount points (`profile`,
  `projects`, `toast`).
- `render.js` fetches `config.json` at page load, applies your theme to CSS
  variables on `:root`, and renders the sidebar + project cards into the
  shell.
- No build step. The browser does the rendering.

## `config.json` schema

```json
{
  "meta": {
    "title": "string  // document title",
    "description": "string  // <meta name=description>",
    "favicon": "string  // path, optional"
  },
  "profile": {
    "name": "string  // required",
    "avatar": "string  // path to portrait image, required",
    "location": "string  // optional, shows under name with pin icon",
    "tagline": "string  // optional, italic one-liner",
    "pitch": "string  // optional, bold sentence",
    "pitchHighlight": "string  // optional, substring of pitch to render as a styled link",
    "bio": "string  // optional, short paragraph"
  },
  "socials": {
    "label": "string  // optional small uppercase heading above the icons",
    "items": [
      { "label": "LinkedIn", "url": "https://...", "icon": "linkedin" }
    ]
  },
  "companies": {
    "label": "string  // optional heading",
    "items": [
      { "name": "Acme", "logo": "assets/companies/acme.svg", "url": "https://...  // optional" }
    ]
  },
  "projects": [
    {
      "name": "string  // required",
      "url": "string  // optional. Omit for non-link cards. Ignored if comingSoon is true.",
      "description": "string",
      "badge": "string  // optional, shown as a pill (e.g. '8 users', 'Beta')",
      "comingSoon": "boolean  // optional. true → clicking shows a toast instead of navigating.",
      "icon": {
        "class": "string  // CSS class for the tile background. Built-ins: ic-gold, ic-purple, ic-green, ic-orange, ic-blue, ic-navy, ic-teal.",
        "glyph": "string  // optional, single character or short text",
        "image": "string  // optional, path to logo image (overrides glyph)",
        "imageAlt": "string  // optional"
      },
      "slides": [
        { "image": "assets/projects/foo/01.png", "label": "Landing" }
      ]
    }
  ],
  "theme": {
    "bg":         "#hex",
    "card":       "#hex",
    "ink":        "#hex  // primary text",
    "ink2":       "#hex  // secondary text",
    "muted":      "#hex",
    "line":       "#hex  // hairline borders",
    "pillBg":     "#hex", "pillFg": "#hex", "pillDot": "#hex",
    "highlight":  "#hex",
    "chart":      "#hex", "chartFill": "rgba(...)",
    "gridDot":    "#hex",
    "dark":       "#hex  // CTAs, toast",
    "pitchLink":  "#hex  // color for pitchHighlight"
  }
}
```

`socials` also accepts a plain array of items if you don't want a label.
Leave `companies` out entirely to hide that row.

### Social icon presets

`icon` field on each social accepts one of:
`linkedin`, `email`, `instagram`, `medium`, `flickr`, `github`, `x`,
`twitter`, `bluesky`, `rss`, `youtube`. Add more in the `ICONS` map at
the top of `render.js`.

### Asset guidelines

- **Portrait**: square, ≥512×512, JPG or PNG.
- **Company logos**: square, ~64×64, rendered at 26px and greyscaled until hover.
- **OG image**: 1200×630 PNG at `assets/og-image.png`.
- **Project slides**: 16:10 aspect ratio (e.g. 1280×800), ≥1280px wide.
  Stored at `assets/projects/<slug>/NN-name.png`. Numeric prefix keeps them
  ordered on disk. Slides are rendered with `object-fit: cover` and
  `object-position: top center`, so keep the important content near the top.

## Theming

Every color in `theme` maps to a CSS custom property on `:root` and is read
by the stylesheet in `index.html`. Change `theme.highlight` to swap the
accent color across the page. To override anything deeper (spacing, fonts,
breakpoints), edit the `<style>` block in `index.html` directly.

## Adding a custom icon-tile class

Built-in tile classes are defined in the `<style>` block in `index.html`,
under "Icon-tile presets". To add your own:

```css
.ic-mine {
  background: linear-gradient(135deg, #f00 0%, #800 100%);
  box-shadow: 0 4px 10px -4px rgba(255,0,0,0.55);
}
```

Then reference it from `config.json`: `"icon": { "class": "ic-mine", "glyph": "M" }`.
If your logo is dark and the tile is dark, add
`.ic-mine img { filter: brightness(0) invert(1); }` to render it white.

## Deploy

The site is static and served from the repo root. There is no build step.

- **Render**: **New → Blueprint** pointed at this repo picks up `render.yaml`,
  or create a Static Site manually with an empty build command and publish
  path `.`. Add a custom domain under **Settings → Custom Domains**.
- **GitHub Pages**: Settings → Pages → deploy from `main`, root folder.

## License

[MIT](./LICENSE). Fork freely, no attribution required.
