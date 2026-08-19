# Museum of Ordinary Life — Possible Future

Production source for [future.museumofordinarylife.org](https://future.museumofordinarylife.org/), a speculative future interface for the Museum of Ordinary Life.

This site is part of the Museum's Reality Layer and the larger *No One Noticed* project. It imagines one possible later interface for the real Museum established in 2026.

## Site files

- `index.html` — complete production page, with its CSS and JavaScript bundled inline
- `favicon.png` — browser icon
- `apple-touch-icon.png` — home-screen icon
- `robots.txt` — crawler policy and sitemap location
- `sitemap.xml` — canonical public URL
- `CNAME` — GitHub Pages custom domain
- `_config.yml` — minimal GitHub Pages configuration
- `DEPLOYMENT.md` — Pages setup, DNS cutover, verification, and rollback runbook

The production files were imported from the Fastmail-hosted site archive on 2026-08-19. At import, all five public files matched the live Fastmail site byte for byte.

## Important

This is a **speculative interface**. Example archive records, locations, interfaces, counts, and future institutional details are fictional unless explicitly identified otherwise. The site should maintain that distinction clearly.

The contribution form is a demonstration. It must remain visibly labeled as non-transmitting unless a real, privacy-reviewed intake service is deliberately added.

## Local preview

From the repository root:

```sh
python3 -m http.server 8000
```

Then open `http://localhost:8000/`. Preview through a web server rather than opening `index.html` directly so asset paths and browser behavior match production.

## Deployment

GitHub Pages publishes from the `main` branch and repository root.

Custom domain: `future.museumofordinarylife.org`

DNS is managed by Fastmail separately from this repository. The website cutover changes only the `future` host record; Fastmail's nameservers and all mail-related MX, SPF, DKIM, and DMARC records stay in place. See [DEPLOYMENT.md](DEPLOYMENT.md) before changing Pages or DNS settings.

## Documentation is part of the change

Every site change must include any related documentation update in the same commit. At minimum, check this README, `DEPLOYMENT.md`, `robots.txt`, `sitemap.xml`, `CNAME`, visible privacy or prototype language, and the file list above. If the deployment method, domain, routes, data handling, or repository structure changes, the documentation is not optional.

## Present-day Museum

The real present-day site is maintained separately in `Museum-of-Ordinary-Life/museumofordinarylife.org`.
