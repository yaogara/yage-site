# Yagé production deployment

- Repository: yaogara/yage-site
- Launch branch: launch/cloudflare-redesign
- Production branch after launch: main
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
- Controlled signup through the Pages preview displayed success and persisted an active subscriber in Baserow table 709. Welcome email delivery remains unverified and must be checked separately; storage success alone does not prove email delivery.
- Legacy index and December 2025 URLs have permanent redirects.
- Canonical and Open Graph URLs use yage.se; sitemap and robots are included.

## Rollback

Before cutover on November 12, 2026, both yage.se and www.yage.se were CNAME records to c96e3b443ba47c0d.vercel-dns-017.com, DNS only, TTL 600. If the new host fails, restore only those web records and retain all email and verification records. Disable the new www redirect rule if restoring the previous routing requires it. Keep a complete DNS snapshot before cutover. The old GitHub Pages source and custom-domain CNAME remain in this repository, but the observed pre-cutover DNS target was Vercel.
