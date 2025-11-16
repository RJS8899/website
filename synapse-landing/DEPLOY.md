# Deploying the Synapse Landing Page

This landing page is completely static and works on any static host. Below are step-by-step guides for popular providers plus notes on adding custom domains and an email backend later.

## 1. Deploy to Netlify
1. Create a Netlify account (or log in) at [https://app.netlify.com](https://app.netlify.com).
2. Click **"Add new site" → "Import an existing project"**.
3. Connect your Git repository containing the `/synapse-landing` folder.
4. Set **Build command** to `npm run build`? No build is necessary, so leave it empty.
5. Set **Publish directory** to `synapse-landing`.
6. Deploy. Netlify will host the static files and automatically apply `_redirects` and `netlify.toml`.
7. To use drag-and-drop: zip the folder locally and drop it onto Netlify's **Deploys** page.

## 2. Deploy to Vercel
1. Install the [Vercel CLI](https://vercel.com/download) or use the dashboard.
2. In the dashboard, click **"Add New..." → "Project"** and import your repository.
3. When prompted for framework preset, choose **"Other"** (static).
4. Set the **Output Directory** to `synapse-landing`.
5. Vercel reads `vercel.json` and serves `index.html` for every route.
6. Deploy and wait for the assigned `.vercel.app` domain to be ready.

## 3. Deploy to Cloudflare Pages
1. Go to [https://dash.cloudflare.com](https://dash.cloudflare.com) → **Pages**.
2. Click **"Create a project"**, connect your Git provider, and select the repo.
3. Leave the build command empty and set the build output directory to `synapse-landing`.
4. Deploy. Cloudflare will detect it’s static and serve the files via its global CDN.
5. For manual uploads, use the **Direct Upload** option and drag-drop the folder contents.

## 4. Deploy to GitHub Pages
1. Commit the `/synapse-landing` folder to your repo main branch.
2. Go to **Settings → Pages** in GitHub.
3. Under **"Build and deployment"**, pick **Deploy from branch**.
4. Select the branch (e.g., `main`) and set the folder to `/synapse-landing`.
5. Save. GitHub Pages will publish the site at `https://<user>.github.io/<repo>/` (or a custom domain).

## 5. Connect a Custom Domain
- **Netlify**: In Site settings → Domain management → Add custom domain. Update your DNS with the provided CNAME or A records. You can optionally edit the `CNAME` file here locally if you plan to use GitHub Pages.
- **Vercel**: Go to Project → Settings → Domains → Add. Point your domain’s DNS to the Vercel-provided names/records.
- **Cloudflare Pages**: In your Pages project → Custom domains → Set up. Add a CNAME record to `<project>.pages.dev`.
- **GitHub Pages**: Add your domain under Settings → Pages and commit the domain name into the `CNAME` file. Point a CNAME to `<user>.github.io` (or A records to GitHub’s IPs).

Always wait for DNS propagation (can take up to 24h) and verify HTTPS certificates are issued.

## 6. Adding an Email Backend Later
The current email form is static. To capture submissions:
- **Formspree**: Create a Formspree project, obtain the form endpoint, and set the form `action` attribute to their URL.
- **Netlify Forms**: Add `data-netlify="true"` to the `<form>` and include a hidden `input name="form-name" value="synapse-waitlist"`. Netlify will collect submissions.
- **Cloudflare Workers / Durable Objects**: Build a worker that handles POST requests and stores emails securely.
- Always validate submissions server-side, store them encrypted, and ensure compliance with privacy regulations.

## 7. Local Preview
1. `cd synapse-landing`
2. Start a simple server, e.g., `python -m http.server 8080`.
3. Visit [http://localhost:8080](http://localhost:8080).

You are ready to share Synapse with the world when the time comes.
