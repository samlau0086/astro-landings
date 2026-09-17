# Astro Landing Page

Mobile-first Astro landing page prepared for local development and Cloudflare Pages deployment.

## Run locally

```bash
npm install
npm run dev
```

## Build

```bash
npm run build
npm run preview
```

## Cloudflare Pages

```bash
npm run cf:preview
npm run cf:deploy
```

The page is configured with `@astrojs/cloudflare` and `output: 'server'`, so it can run on Cloudflare Pages with Workers support while still supporting normal Astro local development.
