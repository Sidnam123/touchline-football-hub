# TOUCHLINE — free football match hub

A beginner-friendly, dark green-accented football website starter. It includes:
- Scores page with live/finished filters
- Upcoming fixtures
- Legal viewing links to official rights-holder information
- Demo data until you connect the API
- Optional Cloudflare Worker proxy so your football-data.org API token is not placed in the public website code

## What you need

- A Mac and a modern browser
- A free GitHub account for publishing the static website
- Optional: free Cloudflare account + football-data.org account/token for real match data

## 1) Run it on your Mac first

1. Unzip `touchline-football-hub.zip`.
2. Open the `football_match_hub` folder.
3. Double-click `index.html`. It should open in your browser.
4. Explore Scores, Fixtures and Watch. The displayed sample scores are demo examples, not real scores.

No install or coding setup is needed to preview the starter.

## 2) Publish the static website for free

1. Sign in at https://github.com and create a **public** repository, e.g. `touchline-football-hub`.
2. Upload `index.html` to the repository root (not inside another folder).
3. Open repository **Settings → Pages**.
4. Under build/deployment, choose **Deploy from a branch**.
5. Choose branch `main` and folder `/ (root)`, then Save.
6. Wait for GitHub Pages to publish; the site URL appears in the Pages section.

GitHub Pages is static hosting. It can serve this HTML/CSS/JavaScript, but it does not run server-side code or provide a live-score API itself. Do not upload API secrets to the repository.

## 3) Optional: connect real fixtures/scores (Cloudflare Worker)

The included `worker.js` is a small backend proxy. It keeps your API token on the server rather than exposing it in the website source. The football-data.org free plan has competition and request limits; confirm the current plan covers the competitions you want.

### A. Get an API token

1. Create an account at https://www.football-data.org/.
2. Follow its instructions to obtain an API token.
3. Do not paste the token into `index.html`, GitHub, a screenshot, or a public issue.

### B. Deploy the Worker

1. Create/sign in to a free Cloudflare account at https://dash.cloudflare.com/.
2. Open **Workers & Pages** and create a Worker.
3. Replace the starter code with the contents of `worker.js` and deploy.
4. In that Worker’s **Settings → Variables and Secrets** (wording may vary), add a secret:
   - Name: `FOOTBALL_DATA_TOKEN`
   - Value: your football-data.org token
5. Deploy again if prompted.
6. Test the Worker URL by opening `https://YOUR-WORKER-URL/matches` in your browser. You should receive JSON. If the API returns an error, check the token, plan access and provider limits.

### C. Point the website to your Worker

1. Open `index.html` in a text editor.
2. Find this line near the bottom:
   `const API_BASE = "";`
3. Replace the empty string with your Worker base URL, without a trailing slash, e.g.:
   `const API_BASE = "https://touchline-api.YOUR-SUBDOMAIN.workers.dev";`
4. Save and upload the changed `index.html` to GitHub. Wait for Pages to redeploy.
5. Refresh the site. The data-status text should say football-data.org and show a refresh time.

The starter requests today's matches through seven days ahead. The returned competitions and data depend on the API provider's current plan and coverage. This is scores/fixture data, not video.

## 4) Legal watch page

The Watch tab links to official league/broadcaster information and an official highlights channel. It does not provide pirate streams or re-host broadcast footage. Broadcast availability varies by country, and live streams may require a paid subscription. Add only video embeds that the rights-holder explicitly permits.

## Important limitations

- Hosting can be free, but a custom domain usually costs money; use the free `github.io` URL.
- API providers can limit leagues, requests, freshness, and commercial use. Read their current terms before launching publicly.
- A free API may not include every competition or truly real-time updates.
- The current website does not auto-refresh continuously. Refresh the page to request fresh data; avoid aggressive polling.
- The included Worker allows cross-origin requests (`*`) for simplicity. For a public production site, restrict `Access-Control-Allow-Origin` to your GitHub Pages URL.
- GitHub Pages is suitable for a static fan project, not a full streaming backend or high-bandwidth video platform.
