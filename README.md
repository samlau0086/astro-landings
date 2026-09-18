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

This landing page uses Astro's static output, which is the simplest and most reliable setup for Cloudflare Pages. The build generates `dist/index.html` and does not require Workers or server-side rendering.

### Local Cloudflare preview

```bash
npm run cf:preview
```

### Manual deployment with Wrangler

Authenticate Wrangler first:

```bash
npx wrangler login
```

Then deploy:

```bash
npm run cf:deploy
```

## GitHub + Cloudflare Pages automatic deployment

The recommended deployment method is Cloudflare Pages Git integration. It automatically builds and deploys the project whenever changes are pushed to the production branch.

Make sure Cloudflare Pages is configured to deploy the branch that contains the latest site changes. This repository currently has a `main` branch and a `site/bags` branch. If the bags landing page is the intended production site, set the Cloudflare production branch to `site/bags`, or merge `site/bags` into `main` before deploying.

### 1. Push the project to GitHub

```bash
git add .
git commit -m "Update landing page"
git push origin main
```

Do not commit `node_modules`, `dist`, `.astro`, `.wrangler`, or `.env` files. These paths are already included in `.gitignore`.

### 2. Connect GitHub to Cloudflare Pages

In Cloudflare:

1. Open **Workers & Pages**.
2. Select **Create application** and then **Pages**.
3. Select **Import an existing Git repository**.
4. Authorize GitHub and choose this repository.
5. Set the production branch to `main`.
6. Use the following build settings:

| Setting | Value |
| --- | --- |
| Framework preset | Astro |
| Build command | `npm run build` |
| Build output directory | `dist` |
| Root directory | Leave empty |

Click **Save and Deploy** to create the first deployment.

### 3. Automatic deployments

After the initial setup, every push to `main` runs:

```text
Install dependencies -> npm run build -> Deploy dist
```

Pull Requests can also receive temporary Cloudflare Pages preview deployments. This allows changes to be reviewed before they are merged into `main`.

### 4. Custom domain

In the Pages project, open **Custom domains**, select **Set up a custom domain**, and follow the DNS instructions.

### 5. Environment variables

Add environment variables in the Pages project under **Settings -> Environment variables**. Configure Production and Preview values separately when needed. Never commit real secrets to GitHub.

## Troubleshooting

Test the production build locally before pushing:

```bash
npm install
npm run build
```

If Cloudflare cannot find the output, verify that the build output directory is exactly `dist`.
