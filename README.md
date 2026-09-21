# Noida Farms · Product Showcase

A one-page showcase of **Noida Farms** (farmhouse renting) for my resume: what the app does, demo
videos for Android and iPhone, the engineering behind it, and download links.

**Live link:** _add after deploying_

---

## Read this first: what still needs your confirmation

This page was written from the project context document, **not from the source code**. Everything on
it is deliberately limited to what that document supports, so nothing here should surprise you in an
interview. But a few things are still worth checking against the repo before you share the link:

| On the page | Status | What to check |
|---|---|---|
| React Native app, admin panel, vendor dashboard | From the context doc | Confirm the admin panel really is React Native, not web |
| Node.js + MongoDB + Mongoose | From the context doc | Confirm |
| PDFKit invoices | From the context doc | Confirm invoices are generated server-side |
| Email / WhatsApp / Google sign-in | From the context doc | Confirm all three shipped |
| Payments and payouts | From the context doc | **No payment gateway is named anywhere on the page** — that was never verified. Add it to `CONFIG.tech` → Integrations once you confirm it |
| Backend framework | **Deliberately absent** | The page says "Node.js API", never Express or Fastify, because the context doc contradicts itself. Add the real one to `CONFIG.tech` → Backend |
| `CONFIG.scope` figures | Structural, safe | 3 surfaces / 3 user types / 3 sign-in methods / 1 backend. All countable facts, not traffic numbers. Swap in real platform numbers when you have them |

**Nothing on this page claims a user count, a booking count, or revenue.** If you add such numbers,
make sure you can source them.

---

## What's on the page

| Section | What it shows |
|---|---|
| Hero | Title, one-line pitch, your name and role, buttons, and the app screenshot |
| Demo | Android and iPhone screen recordings in phone frames |
| Scope | Four structural figures in a dark band, from `CONFIG.scope` |
| Features | Six feature cards |
| What I built | Six engineering highlights and an architecture diagram |
| Tech | The stack, grouped into Mobile, Admin & vendor, Backend and Integrations |
| Collabs | Instagram reels, if you add any |
| Code | GitHub repositories |
| Download | App Store / Google Play / website buttons |

Plain HTML, CSS and JavaScript in one file, no build step. The only outside resources are Google
Fonts and Instagram's embed script, which loads only if there are reels to show.

---

## Folder structure

```
showcase/
├── index.html            # the whole page: HTML, CSS and JS
├── README.md
└── assets/
    ├── logo.svg          # PLACEHOLDER — replace with the real logo (see below)
    ├── images/
    │   └── hero.png      # you add this
    └── videos/
        ├── android.mp4   # you add this
        └── ios.mp4       # you add this
```

---

## The logo is a placeholder

`assets/logo.svg` is my approximation of the Noida Farms mark, drawn from the splash screen — the
bold left roof-and-column with the fine right outline. **Replace it with the official asset.**

It's used in four places: the nav, the footer, the browser tab icon, and the phone placeholders.
The footer and phone placeholders apply `filter: brightness(0) invert(1)` to render it white on
dark, so a **transparent** PNG or SVG works best. If you swap in a `.png`, update the three
`assets/logo.svg` references in `index.html` and the two `<link rel="icon">` tags.

The page's typography (Cinzel) was chosen to match the logo's Roman serif wordmark, and the hero
deliberately renders "Noida" in ink and "Farms" in brand green to mirror the logo lockup.

---

## Colours

Taken straight from the app's `COLORS` constant and declared at the top of `index.html`:

```
primary #2DC553 · primaryDark #257B00 · primaryLight #EDFAF0
background #FFFFFF · text #252A32 · textSecondary #666666 · border #E5E5E5 · danger #E53935
```

**One rule worth keeping:** `#2DC553` is only 2.3:1 against white, which fails accessibility
contrast for text. On this page it is used as a *fill* — icons, glows, gradients, numbers on the
dark band — and never as text. `#257B00` is 5.4:1 and carries every green label, link and button.
If you restyle, keep that split.

---

## Preview locally

Serve the folder over HTTP. Opening the file directly mostly works, but Instagram embeds need a server.

```bash
cd ~/Desktop/NoidaFarms/showcase && python3 -m http.server 4321
```

Then open <http://localhost:4321>.

**On localhost, empty sections show placeholders** that say what's missing (for example "Add your
video at assets/videos/ios.mp4"). **Once deployed, empty sections hide themselves**, so visitors
never see a half-finished page. That applies to Collabs, Code, Scope, the Integrations tech card,
and the whole Download section if no store links are set.

---

## Editing: everything is in `CONFIG`

Open `index.html` and find `const CONFIG = {` near the bottom. You shouldn't need to touch the HTML or CSS.

| Field | What it does |
|---|---|
| `name`, `role` | Shown in the hero, footer and browser tab |
| `contact.email` / `linkedin` / `github` | Icon links in the hero and text links in the footer. Leave a field empty to hide it |
| `website`, `playStore`, `appStore` | **Currently blank.** Every store button, demo-section link and footer link reads from these |
| `hero` | Hero image. `framed: true` wraps a plain screenshot in a phone frame; `framed: false` if the image already contains the phone (use a transparent PNG) |
| `videos.android` / `videos.ios` | `src` (path or URL to an .mp4) and an optional `poster` image |
| `scope` | The four figures in the dark band |
| `tech` | The stack chips, grouped. An empty group hides itself once deployed |
| `instagram` | List of reels, each `{ url, label }`. Must be public to embed |
| `github` | List of repos. Set a `url`, or `private: true` for "Private repo · walkthrough on request" |

### Store links

All three are blank right now, so the page shows dashed "Coming soon" buttons on localhost and
hides the Download section entirely once deployed. Paste the URLs in and everything lights up at
once — the hero website button, the Android/iPhone links under the demos, the footer and the
download buttons all read from the same three fields.

---

## Demo videos

Put recordings at `assets/videos/android.mp4` and `assets/videos/ios.mp4`, or point
`CONFIG.videos.*.src` at any hosted .mp4.

- **Orientation is detected automatically** — portrait gets a phone frame, landscape a wide frame.
- **Playback:** muted and looping while on screen, paused when scrolled away. The expand button goes
  full screen **with sound**.
- **If a file is missing**, the frame shows a placeholder, so the layout never breaks.

Shrink a screen recording to a web-friendly size:

```bash
ffmpeg -i recording.mov -vf "scale=-2:1280" -c:v libx264 -crf 26 -preset slow -movflags +faststart -an android.mp4
```

`-an` removes audio. Aim for **under ~10 MB per video**; GitHub rejects files over 100 MB.

---

## Deploy

Any static host works.

**GitHub Pages:**

```bash
cd ~/Desktop/NoidaFarms/showcase && git init && git add . && git commit -m "Noida Farms showcase"
```

Then add a remote, push, and set **Settings → Pages → Deploy from a branch → `main` / root**.

**Netlify Drop:** drag the `showcase` folder onto <https://app.netlify.com/drop>.
**Vercel:** run `npx vercel --prod` inside this folder.

Add a `.gitignore` containing `.DS_Store` before the first commit.

---

## Accessibility

The page respects "reduce motion" (no autoplay, count-ups or reveal animations), works with the
keyboard, and every colour pairing meets WCAG AA. Verified responsive down to 375px.

---

## Before sharing: checklist

- [ ] Replace `assets/logo.svg` with the official logo
- [ ] Work through the confirmation table at the top of this file
- [ ] `name`, `role` and contact links are correct
- [ ] Hero screenshot added at `assets/images/hero.png` (no real customer names or photos)
- [ ] `android.mp4` and `ios.mp4` added and compressed
- [ ] Store links added, or Download section intentionally left hidden
- [ ] GitHub repos linked, or marked `private: true`
- [ ] Deployed and tested on a phone
- [ ] Link added to the resume
