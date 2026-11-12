# Yagé production deployment

- Repository: yaogara/yage-site
- Launch branch: launch/cloudflare-redesign
- Production branch: main (automatic deployments enabled)
- Pages project: yage-site (https://yage-site.pages.dev)
- Framework preset: None
- Build command: leave blank
- Build output directory: dist
- Root directory: repository root
- Canonical domain: https://yage.se

Create the Pages project against the launch branch and verify its pages.dev deployment before moving DNS. Attach yage.se and www.yage.se in Pages first. Configure a Cloudflare Single Redirect matching hostname www.yage.se and redirecting to https://yage.se with the path and query string preserved (301). Pages `_redirects` accepts only relative source URLs and cannot implement this hostname redirect. The apex requires the domain to use Cloudflare nameservers. Preserve all existing MX, TXT, DKIM, SPF, DMARC, and other email records when moving DNS. Keep the previous hosting configuration available until the Cloudflare custom domains are Active and HTTPS works.

The original root-level website stays on main during preparation. Its pre-migration commit is c40c321659501c6988e3cdb18279658eefa22690. Do not merge until the Cloudflare preview is verified; merge only after cutover when source control is ready. Configure Pages production branch main after merge.

## Checks

- JavaScript syntax and local asset references checked.
- English / Swedish toggle, animation pause/resume and newsletter iframe checked in browser.
- Controlled signup through the Pages preview displayed success and persisted an active subscriber in Baserow table 709. Welcome email delivery was repaired and verified through the live domain on November 12, 2026; see Newsletter below.
- Legacy index and December 2025 URLs have permanent redirects.
- Canonical and Open Graph URLs use yage.se; sitemap and robots are included.

## Rollback

Before cutover on November 12, 2026, both yage.se and www.yage.se were CNAME records to c96e3b443ba47c0d.vercel-dns-017.com, DNS only, TTL 600. If the new host fails, restore only those web records and retain all email and verification records. Disable the new www redirect rule if restoring the previous routing requires it. Keep a complete DNS snapshot before cutover. The old GitHub Pages source and custom-domain CNAME remain in this repository, but the observed pre-cutover DNS target was Vercel.

## Launch verification — November 12, 2026

PR #1 was merged as 31f51049ade66b08838c542c084f1b7767dc6bd6 after the Pages preview and live-domain checks. Both custom domains are Active with SSL enabled. The apex and www CNAMEs point to yage-site.pages.dev, proxied, TTL Auto. All seven other DNS records were preserved, including MX, DKIM, SPF, and Vercel verification TXT records.

The enabled Single Redirect `Yagé www to canonical apex` (rule ID 4a6db218370b4dd4a363b31385ac289c) matches `*://www.yage.se/*`, redirects to `https://yage.se/${2}` with status 301, and preserves query strings. HTTP apex requests redirect to HTTPS. Legacy links, static assets, robots, sitemap, and custom 404 behavior passed HTTP checks on the live domain. Controlled signup through the live domain displayed success and persisted an active subscriber in Baserow. Automatic welcome email delivery is verified; see Newsletter below.

## Newsletter — verified November 12, 2026

The Baserow form creates subscribers in table 709. Its rows.created webhook triggers n8n workflow `Yage - Welcome Emails` (OClrdMkKIp5uZv64). The workflow uses existing credential `Mailersend API - July 2026` and sends a bilingual English/Swedish welcome from aya@yage.se with subject `Welcome to Yagé / Välkommen till Yagé`, then sets welcomeSent in Baserow. Content is stored directly as HTML/text in the email node because the previous provider template was missing and creating provider templates required a paid plan. Do not restore obsolete template ID x2p0347oy07gzdrn or the old embedded API key.

A controlled live-domain signup completed all three nodes in execution 6151, set welcomeSent true, and delivered the welcome email to the controlled Gmail inbox. The white-background update is published as workflow version a5d81e37-6c0a-4cc1-b8d0-f6533d22d7ee. Secrets remain in n8n credentials, not in this repository. Unsubscribe instructions direct recipients to reply to aya@yage.se.

The welcome email HTML is versioned at [`docs/emails/yage-welcome-confirmation.html`](docs/emails/yage-welcome-confirmation.html). Its background is plain white (`#ffffff`) and matches the live n8n email payload. Editing this file does not update n8n automatically; sync its HTML into the email node and publish the workflow when changing email content.
