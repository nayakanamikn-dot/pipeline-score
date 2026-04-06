# Pipeline Score

**Free B2B pipeline readiness audit tool.** 10 questions, 60 seconds, custom action plan.

Built as a lead generation tool — founders take the quiz, see where their pipeline leaks, and book a strategy call to fix it.

![Pipeline Score](https://img.shields.io/badge/status-production-green) ![Deploy](https://img.shields.io/badge/deploy-Vercel-black)

---

## What It Does

1. **Intro screen** — hooks the visitor with a clear value prop ("Find your pipeline leaks in 60 seconds")
2. **10-question quiz** — scores across 5 categories: LinkedIn Presence, Cold Outreach, CRM & Follow-up, Content System, Pipeline Predictability
3. **Email capture gate** — collects name + email before revealing results (lead generation)
4. **Results dashboard** — animated score ring, category breakdown with color-coded bars, prioritized 3-step action plan, CTA to book a call, social sharing

## Tech Stack

- **Pure HTML/CSS/JS** — zero dependencies, zero build step, instant load
- **Vercel** — static hosting with edge caching and security headers
- **Google Fonts** — DM Sans + JetBrains Mono with `font-display: swap`

## Project Structure

```
pipeline-score/
├── public/
│   ├── index.html          # Main HTML (SEO meta, OG tags, structured data)
│   ├── css/
│   │   └── styles.css      # All styles (responsive, print, a11y)
│   ├── js/
│   │   ├── questions.js    # Quiz data (questions, categories, recommendations)
│   │   └── app.js          # App logic (quiz engine, email capture, analytics)
│   └── assets/
│       └── favicon.svg     # Favicon
├── vercel.json             # Vercel deployment config + security headers + caching
├── .env.example            # Environment variable template
├── .gitignore
└── README.md
```

## Run Locally

No build step needed. Just serve the `public/` folder:

```bash
# Option 1: Python
cd public && python3 -m http.server 3000

# Option 2: Node (npx)
npx serve public

# Option 3: VS Code
# Install "Live Server" extension, right-click index.html → Open with Live Server
```

Then open `http://localhost:3000`

## Deploy to Vercel

### First Time
1. Push this repo to GitHub
2. Go to [vercel.com/new](https://vercel.com/new)
3. Import the GitHub repo
4. **Framework Preset:** Other
5. **Output Directory:** `public`
6. Click Deploy

### Subsequent Deploys
Every push to `main` branch auto-deploys via Vercel's GitHub integration.

### Custom Domain
1. Go to your Vercel project → Settings → Domains
2. Add your domain (e.g., `pipelinescore.co`)
3. Update DNS records as instructed
4. Update `SITE_URL` in `.env` and OG meta tags in `index.html`

## Environment Variables

Copy `.env.example` to `.env` and configure:

| Variable | Required | Description |
|---|---|---|
| `GA_MEASUREMENT_ID` | No | Google Analytics 4 tracking ID |
| `BOOKING_URL` | No | Calendly or Cal.com booking link |
| `CONTACT_EMAIL` | No | Email for CTA (default: nayak.anamik.n@gmail.com) |
| `SITE_URL` | No | Production URL for OG tags |
| `WEBHOOK_URL` | No | Zapier/Make/n8n webhook for lead capture |

**Note:** Since this is a static site, env vars are currently hardcoded in the HTML/JS files. To use dynamic env vars, integrate a build step (e.g., Vite) or use Vercel Edge Functions.

## Analytics

The app tracks these events (works with GA4 and Vercel Analytics):

| Event | When |
|---|---|
| `page_loaded` | Page opens |
| `quiz_started` | User clicks "Check My Pipeline Score" |
| `option_selected` | User selects an answer |
| `capture_shown` | Email capture screen appears |
| `lead_captured` | User submits name + email |
| `results_shown` | Results page renders |
| `share_clicked` | User clicks LinkedIn/Twitter/Copy |

### Enable Google Analytics
1. Create a GA4 property at [analytics.google.com](https://analytics.google.com)
2. Get your Measurement ID (G-XXXXXXXXXX)
3. Add this to `index.html` `<head>`:
```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

### Enable Vercel Analytics
1. In your Vercel dashboard → Project → Analytics → Enable
2. Analytics auto-injects on Vercel-hosted deployments

## Lead Capture

Captured leads are currently stored in `localStorage`. To send them to your CRM:

### Option 1: Webhook (Zapier/Make/n8n)
Add a `fetch()` call in the `storeLead()` function in `app.js`:
```javascript
fetch('YOUR_WEBHOOK_URL', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(lead)
});
```

### Option 2: HubSpot Forms API
Replace the webhook URL with HubSpot's form submission endpoint.

### Option 3: Google Sheets
Use a Google Apps Script web app as the webhook endpoint.

## Performance

- **Zero dependencies** — no npm, no build, no framework overhead
- **~15KB total** (HTML + CSS + JS, before gzip)
- **Font optimization** — preconnect + display:swap prevents FOIT
- **Static caching** — CSS/JS/assets cached for 1 year via Vercel headers
- **Security headers** — X-Content-Type-Options, X-Frame-Options, XSS Protection, Referrer-Policy

## Accessibility

- Semantic HTML with ARIA roles and labels
- Keyboard navigation (arrow keys, Enter, Space)
- Focus-visible outlines
- Color contrast meets WCAG AA
- Print stylesheet included

## Browser Support

- Chrome 80+
- Firefox 78+
- Safari 13+
- Edge 80+
- Mobile Safari / Chrome (iOS/Android)

---

## License

MIT

## Author

**Anamika Nayak** — [LinkedIn](https://www.linkedin.com/in/anamikanayak) | nayak.anamik.n@gmail.com
