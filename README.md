# Sesil & Ariya Wedding Invitation

Static single-page wedding invitation (Bahasa Indonesia) built with Tailwind CSS (CDN) and vanilla JavaScript. The page presents an immersive opening splash, event details, gallery, undangan digital form, serta integrasi webhook untuk ucapan tamu.

## Features

- Splash screen with progress preload and optional background music.
- Personalized guest greeting via `?to=Nama+Tamu` query string.
- Event timeline, countdown, venue map shortcut, and add-to-calendar (`.ics`) export.
- Hero cover video with graceful fallback to images.
- Gallery lightbox and photo/video assets in `public/assets`.
- Wishes board that syncs with an n8n webhook and falls back to local storage when offline.
- Digital envelope section with copy-to-clipboard gift accounts.
- Toast notifications and subtle scroll-based reveal animations.

## Project Structure

```
.
├── index.html        # Main invitation page (all logic/scripts inline)
└── public/
    ├── assets/       # Video, audio, and gallery assets
    └── favicon/      # Generated favicon set + manifest
```

_All logic lives in `index.html`, so edits happen in-place._

## Getting Started

1. Clone or download the project files.
2. Serve locally:
   - Simple option: open `index.html` directly in a browser.
   - Recommended (correct audio/video + fetch behavior): run any static server, e.g.
     ```bash
     python3 -m http.server 8080
     ```
     then browse to `http://localhost:8080/index.html`.
3. To share a personalized link, append `?to=Nama+Tamu` (URL encoded) to the URL.

## Personalization Checklist

- **Core metadata**: Update `<title>` and favicon references if needed.
- **Config block** (`<script>` near the bottom):
  - `const config` — edits for couple names, event times, venue, preload assets.
  - `const WEBHOOK_BASE` & `WISHES_ENDPOINT` — point to your n8n (or other) backend endpoint for GET/POST wishes.
  - `const DISABLE_MUSIC` — flip to `true` to silence autoplay.
- **Texts & sections**: All copy is inline in the HTML; search for the relevant section headers (Hero, Story, Gallery, RSVP, Gift) and adjust wording.
- **Accounts (Amplop Digital)**: Replace account numbers and labels inside the “Amplop Digital” section.
- **Assets**: Drop new media into `public/assets` and update `<img>`/`<video>` sources accordingly.

> Tips: Use consistent resolution/ratio for gallery images; compress assets for faster loads. Preload URLs in `config.assetsToPreload` should be publicly accessible for the splash progress bar.

## Wishes Workflow

- **Live mode**: Submits to `WISHES_ENDPOINT` (expects JSON response). Ensure CORS for the invitation’s origin.
- **Offline fallback**: If webhook fails, wishes queue in `localStorage` and UI displays cached entries.
- **Rate limiting**: Basic 5-second guard (`lastSubmitAt`) prevents rapid spam.

To disable remote posting entirely (offline demo), set a dummy endpoint and allow local storage to handle the list.

## Add-to-Calendar

Clicking “Save the Date” triggers `downloadICS()` which generates a 3-hour block from `config.dateISO`. Adjust the duration or file name in the same function if your schedule differs.

## Deployment

Any static hosting works (GitHub Pages, Netlify, Vercel, S3). Ensure:

- All `/public` assets are published with the root HTML.
- If using a custom domain with HTTPS, confirm webhook endpoints allow secure requests.
- Configure service worker/CDN caching if you want aggressive asset caching.

## Acknowledgements

- Google Fonts: [Ovo](https://fonts.google.com/specimen/Ovo).
- Tailwind CSS via CDN.
- Unsplash images used in preload array (replace with licensed assets for production).
- Audio track `sampai jadi debu` stored locally in `public/assets`.

Feel free to adapt the content or structure to match your wedding story.
