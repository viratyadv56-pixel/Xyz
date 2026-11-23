# XYZ Realty Owner Desk

A single-page owner dashboard for the XYZ Realty voice-agent project. It uses HTML, CSS and vanilla JavaScript, connects directly to Supabase for authentication/data, and calls the configured n8n outbound-call webhook.

## Deploy to Render (Blueprint)

1. Upload this project folder to a GitHub repository. Keep `render.yaml` at the repository root and `index.html` at `public/index.html`.
2. In Render, choose **New → Blueprint**, connect the repository, and apply the detected Blueprint.
3. Render serves the `public` directory as a static site. No build command or environment variables are required for the current file.
4. Open the generated `https://<service-name>.onrender.com` URL directly in a browser (not embedded in another site).

## Manual Render setup

Choose **New → Static Site**, connect the repository, set:

- Build command: leave blank
- Publish directory: `public`

If using manual setup, add the security headers from `render.yaml` in the Render dashboard. The HTML also contains a Content Security Policy meta tag; the response header adds `frame-ancestors 'none'`.

## Before using the live dashboard

- Confirm the Supabase project URL and publishable key in `public/index.html` are the intended project values.
- Confirm Supabase Row Level Security (RLS), policies, and the `dashboard_access()` function are correctly configured. The browser key is public; RLS and authentication must enforce access.
- Confirm the n8n webhook validates the authenticated owner, consent, and do-not-call status server-side. Keep outbound calling in dry-run/review mode until tested and explicitly approved.
- The dashboard's Content Security Policy `connect-src` must include the exact Supabase and n8n origins used by the app. Update both the meta policy and Render header if you change the policy design.
- The app uses external Google Fonts and jsDelivr for the Supabase browser library, so those domains must remain allowed by the page's policy.
- Test login, data loading, creating/updating records, session expiry, mobile layout, and outbound-call dry-run before production use.

## Local preview

From the project root:

```bash
python3 -m http.server 8080 -d public
```

Then open `http://localhost:8080`.

## Notes

This is a static site; Render hosts the frontend only. Supabase and n8n remain separate services. The proposal and payment receipt PDFs supplied alongside the HTML are reference documents, not website assets, so they are not included in this deployment package.
