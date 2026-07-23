# praxa.page

The marketing + legal site for **Praxa**. Plain static HTML/CSS — no framework, no
build step, no JavaScript. Hosted on **GitHub Pages** behind the Porkbun domain
`praxa.page`.

## Pages

| URL | File |
| --- | --- |
| `praxa.page/` | `index.html` — landing hero |
| `praxa.page/privacy` | `privacy/index.html` — privacy policy |
| `praxa.page/about` | `about/index.html` — placeholder |

The privacy policy content is the HTML twin of `app_demo_1/docs/legal/privacy-policy.md`.
**If you edit the policy, update both.**

Paths are **absolute** (`/style.css`) because the site is served from the domain root.
To preview locally you must serve it over HTTP (not open the files directly, or absolute
paths 404):

```bash
python3 -m http.server 8137   # then open http://localhost:8137/
```

## Assets & how to regenerate them

- **Favicons / touch icon** (`favicon-16.png`, `favicon-32.png`, `apple-touch-icon.png`)
  are downscaled from the app icon. Regenerate if the app icon changes:
  ```bash
  ICON=../app_demo_1/assets/images/icon.png   # adjust path
  sips -z 180 180 "$ICON" --out apple-touch-icon.png
  sips -z 32 32 "$ICON" --out favicon-32.png
  sips -z 16 16 "$ICON" --out favicon-16.png
  ```
- **App Store badges** (`app-store-badge.svg` black, `app-store-badge-white.svg` white)
  are Apple's official artwork (black on light, white on dark, swapped via `<picture>`).
  Do not hand-edit them. Re-fetch from Apple's badge endpoint if needed.
- **`og-image.png`** (1200×630 link-preview banner) is rendered from `_src/og-image.html`.
  Regenerate by serving the site and screenshotting that file at a 1200×630 viewport
  (deviceScaleFactor 1) with a headless browser.

## Deploy (GitHub Pages + Porkbun)

1. Push this directory to a **public** GitHub repo.
2. Repo → **Settings → Pages**: Source = `main` / root. Set **Custom domain** to
   `praxa.page` (the committed `CNAME` already declares it).
3. **Porkbun DNS** for `praxa.page`:
   - Apex `A` records → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`,
     `185.199.111.153`
   - `CNAME` host `www` → `<your-github-username>.github.io` (gives the www → apex
     redirect)
4. Once DNS resolves and the cert issues, tick **Enforce HTTPS**.
5. Verify `https://praxa.page/privacy` in an incognito window; confirm `www.praxa.page`
   redirects to the apex.

## Notes

- `_review/` holds local QA screenshots — safe to delete; not needed to deploy.
- Canonical host is the **apex** `praxa.page` (the in-app `PRIVACY_POLICY_URL` points
  here). Keep them in sync if the domain ever changes.
