# GitHub Pages deployment and DNS cutover

This runbook moves only `future.museumofordinarylife.org` from Fastmail website hosting to GitHub Pages. It does not move DNS hosting or email away from Fastmail.

## Target configuration

- Repository: `Museum-of-Ordinary-Life/future.museumofordinarylife.org`
- Pages source: `main` branch, repository root (`/`)
- Custom domain: `future.museumofordinarylife.org`
- Required DNS target: `museum-of-ordinary-life.github.io.`

The repository-root `CNAME` file must contain only `future.museumofordinarylife.org`.

## Observed pre-cutover state

Checked on 2026-08-19:

- `future.museumofordinarylife.org` resolves to Fastmail web-hosting addresses `103.168.172.52` and `103.168.172.37` with an approximately one-hour TTL.
- The live HTTPS site is served by Fastmail and matches the imported production bundle.
- GitHub Pages was not yet enabled when the migration began.
- `future.museumofordinarylife.com` does not resolve and is not part of this migration.
- The apex domain continues to use Fastmail nameservers and Fastmail mail delivery.

Mail records are outside this cutover. The observed mail configuration includes:

- apex MX: `us1-smtp.messagingengine.com` and `us2-smtp.messagingengine.com`
- apex SPF: `v=spf1 include:spf.messagingengine.com ?all`
- DKIM selectors: `fm1._domainkey`, `fm2._domainkey`, and `fm3._domainkey`
- DMARC: `_dmarc`
- Fastmail subdomain-mail MX at `mail.museumofordinarylife.org`

Do not delete, replace, or duplicate any of those records during the website move.

## Prepare GitHub Pages before DNS

1. Publish the production bundle and this documentation to `main`.
2. In the repository's **Settings → Pages**, choose **Deploy from a branch**, `main`, and `/ (root)`.
3. Set the custom domain to `future.museumofordinarylife.org`. The committed `CNAME` file should already supply it.
4. Wait for the Pages build to succeed.
5. Recommended: in the Museum organization’s **Settings → Pages**, verify `museumofordinarylife.org`. Add only the GitHub-provided `_github-pages-challenge-...` TXT record in Fastmail DNS and leave it in place. This protects the domain from Pages takeover.
6. Verify the GitHub-hosted copy before changing public DNS. Compare the HTML, icons, robots policy, sitemap, internal anchors, dialogs, theme control, archive filtering, and demonstration form against the current Fastmail site.

Do not point DNS at GitHub before the repository claims the custom domain.

## DNS cutover in Fastmail

If Fastmail permits TTL changes, lower the `future` record TTL in advance and wait at least one old TTL before continuing. The observed TTL is already about one hour, so this is optional.

Change only the website record named `future`:

1. Remove the two `A` records for `future` that currently point to `103.168.172.52` and `103.168.172.37`.
2. Add one `CNAME` record:

   - Name/host: `future`
   - Target/value: `museum-of-ordinary-life.github.io.`

A hostname cannot have both a `CNAME` and `A` records, so the two old `A` records must be removed as part of the same focused change.

Do not change the domain's nameservers, apex records, MX records, SPF, DKIM, DMARC, mail subdomains, or any unrelated website record.

## Verify after cutover

Check from more than one resolver or network until all of the following are true:

- `future.museumofordinarylife.org` returns a `CNAME` to `museum-of-ordinary-life.github.io.`
- `https://future.museumofordinarylife.org/` loads the correct site without certificate warnings.
- HTTP redirects to HTTPS after **Enforce HTTPS** is enabled in Pages settings.
- `favicon.png`, `apple-touch-icon.png`, `robots.txt`, and `sitemap.xml` return HTTP 200.
- The present-day Museum link, internal navigation, dialogs, color themes, archive search/filter, and demo form still work.
- Mail to an existing `@museumofordinarylife.org` address still sends and receives normally.

GitHub may need time to provision the custom-domain certificate after DNS changes. Keep the custom domain configured, wait for Pages to show the certificate as available, then enable **Enforce HTTPS**. Do not treat the migration as complete until HTTPS works without bypassing certificate checks.

## Rollback

Keep the Fastmail website-hosting copy intact until GitHub Pages has been stable for at least 48 hours.

If the GitHub site or certificate fails during the cutover:

1. Remove the `future` CNAME to `museum-of-ordinary-life.github.io.`.
2. Restore the two previous `future` A records: `103.168.172.52` and `103.168.172.37`.
3. Wait for DNS caches to expire, then confirm the Fastmail copy is serving again.
4. Leave all mail records unchanged.

After a successful rollback, diagnose Pages separately before attempting another DNS change.

## Completion criteria

The migration is complete only when the Pages build is successful, DNS points `future` to GitHub, HTTPS is enforced and valid, the production files and interactions pass verification, Fastmail email still works, and the rollback copy has been retained through the observation window.
