# Hexy's Bot Manager, Website

Live site: https://hexysbot.netlify.app/

This is the marketing and download site for Hexy's Bot Manager, a desktop
dashboard for running and managing a Discord bot. It's a static site: plain
HTML, CSS, and vanilla JavaScript, no build step and no framework required.

## Pages

| Page | File | What it does |
|---|---|---|
| Home | `index.html` | Full-screen moon photo hero, animated title, and the Download button (with its launch animation) |
| Photos | `photos.html` | A little animated solar system where each orbiting planet is a screenshot of the app. Hover a planet to preview it, click for the full photo |
| Download Plugins | `plugins.html` | Coming soon page for a future plugin download hub |
| Notes | `notes.html` | Full documentation: every feature explained, plus an honest breakdown of what data the app stores locally and where |

## Folder structure

```
hexys-site/
├── index.html
├── notes.html
├── photos.html
├── plugins.html
├── assets/
│   └── moon.webp              the hero background photo
├── photos/
│   ├── dashboard.png          planet photo #1
│   ├── chat-viewer.png        planet photo #2
│   ├── moderation.png         planet photo #3
│   ├── servers.png            planet photo #4
│   ├── debug-mode.png         planet photo #5
│   └── download-page.png      planet photo #6
└── downloads/
    └── hexys-bot-manager.zip  the actual app download
```

## Updating things

**Swap in real screenshots.** The six files in `photos/` are placeholders.
Replace them with your own screenshots using the exact same filenames and
the Photos page picks them up automatically, no code changes needed.

**Ship a new build of the app.** Replace
`downloads/hexys-bot-manager.zip` with the updated zip, keeping that same
filename. The Download button on the home page always points at that path.

**Change any of the copy.** Every page is a single self-contained HTML
file with its CSS and JavaScript inline, so there's nothing else to hunt
down. Open the page, find the text, edit it.

## Running it locally

No install needed. From this folder:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000` in a browser.

## Deploying

This repo is already set up to deploy as-is on Netlify, GitHub Pages, or
any other static host, since there's no build step. Point the host at the
root of this folder and it works.

## Notes on the placeholder content

The six images in `photos/` were generated as stand-ins so the Photos page
launches fully populated instead of empty. Replace them with real
screenshots whenever you're ready.
