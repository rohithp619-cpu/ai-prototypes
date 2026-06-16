# FlexHaus — The Comeback

A three.js scroll-driven booking page for FlexHaus Gym's member-retention campaign.

## Run locally

```bash
# Serve the /site directory with any static file server
npx serve .       # or
python3 -m http.server 8000
# Open http://localhost:8000
```

## Deploy to GitHub Pages

1. Push the `/site` directory to a GitHub repo
2. Go to **Settings → Pages** and set source to the folder containing `index.html`
3. Or use `gh-pages`:
   ```bash
   npm install -g gh-pages
   gh-pages -d site
   ```
4. Live URL will be `https://<username>.github.io/<repo>/`

## Form

The booking form submits to Formspree (free tier). To use your own endpoint:
1. Create a free Formspree form at https://formspree.io
2. Replace the `action` URL in the `fetch` call in `index.html`
3. The form also falls back to `mailto:` if Formspree is unreachable

## Performance budget

- Three.js r169 via CDN importmap (~150KB gzipped)
- UnrealBloomPass for Volt glow
- ~3,000 GPU particles for chalk dust
- Target: < 2.5s interactive on mid-range phone

## Accessibility

- `prefers-reduced-motion`: serves static layout
- No-WebGL fallback: serves static layout with full booking form
- Screen-reader friendly with `aria-hidden` canvas and real DOM content
- Keyboard-navigable with visible focus rings
- AA contrast on all text/background combinations
