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

### 6. Google Analytics 4

This site supports Google Analytics 4 (GA4) for traffic analysis, page-view tracking, and measuring clicks on WhatsApp, email, catalog, and discount CTAs.

#### Create a GA4 property

1. Open [Google Analytics](https://analytics.google.com/) and sign in with the Google account used for the business.
2. Open **Admin**.
3. Select **Create** -> **Property**.
4. Enter the website or business name.
5. Select the appropriate reporting time zone and currency.
6. Complete the property setup.

#### Create a web data stream

1. In the new property, open **Admin** -> **Data collection and modification** -> **Data streams**.
2. Select **Web**.
3. Enter the production website URL, for example `https://your-domain.com`.
4. Enter a stream name such as `MAESVANTI Website`.
5. Select **Create stream**.
6. Copy the **Measurement ID**, which looks like `G-XXXXXXXXXX`.

#### Configure Cloudflare Pages

1. Open the project in **Cloudflare Dashboard**.
2. Go to **Workers & Pages** -> select the Pages project.
3. Open **Settings** -> **Environment variables**.
4. Add a variable for the **Production** environment:

| Variable | Value |
| --- | --- |
| `PUBLIC_GA_MEASUREMENT_ID` | `G-XXXXXXXXXX` |

5. Save the variable.
6. Trigger a new deployment so the measurement ID is included in the generated site.

For preview deployments, add a separate preview value only if preview traffic should also be tracked. Otherwise, leave the Preview environment unconfigured to avoid mixing test traffic with production data.

#### Configure local testing

Copy `.env.example` to `.env` and add the measurement ID:

```text
PUBLIC_GA_MEASUREMENT_ID=G-XXXXXXXXXX
```

Start the local site with `npm run dev`. The `.env` file is ignored by Git and must not be committed.

#### Verify that tracking works

1. Open the deployed production website in a new browser tab.
2. In Google Analytics, open **Reports** -> **Realtime**.
3. Wait a few seconds and confirm that an active user appears.
4. Click a WhatsApp, email, catalog, or discount button.
5. In Realtime, open the event activity to confirm the corresponding event name.

The configured event names include:

| User action | Event name |
| --- | --- |
| Hero WhatsApp catalog button | `whatsapp_catalog_hero` |
| Category catalog button | `whatsapp_catalog_categories` |
| Promotion discount button | `whatsapp_discount_promotion` |
| Wholesale email button | `email_wholesale_inquiry` |
| Contact WhatsApp button | `whatsapp_catalog_contact` |
| Footer WhatsApp button | `whatsapp_catalog_footer` |

#### View traffic reports

- **Realtime:** See visitors currently on the site.
- **Reports** -> **Acquisition** -> **Traffic acquisition:** See where visitors came from.
- **Reports** -> **Engagement** -> **Events:** Review page views and CTA click events.
- **Reports** -> **Engagement** -> **Landing page:** Compare traffic and engagement by entry page.

New data may take 24-48 hours to appear in standard reports. Realtime reports are usually the quickest way to confirm a new deployment.

## Troubleshooting

Test the production build locally before pushing:

```bash
npm install
npm run build
```

If Cloudflare cannot find the output, verify that the build output directory is exactly `dist`.
