# Yagé production deployment

- Repository: yaogara/yage-site
- Launch branch: launch/cloudflare-redesign
- Framework preset: None
- Build command: leave blank
- Build output directory: dist
- Root directory: repository root
- Canonical domain: https://yage.se

Create the Pages project against the launch branch and verify its pages.dev deployment before moving DNS. Attach yage.se and www.yage.se in Pages first; deployed _redirects sends www to yage.se. The apex requires the domain to use Cloudflare nameservers. Preserve all existing MX, TXT, DKIM, SPF, DMARC, and other email records when moving DNS. Do not disconnect GitHub Pages until the Cloudflare custom domains are Active and HTTPS works.

The original root-level website stays on main during preparation. Its pre-migration commit is c40c321659501c6988e3cdb18279658eefa22690. Do not merge until the Cloudflare preview is verified; merge only after cutover when source control is ready. Configure Pages production branch main after merge.

## Checks

- JavaScript syntax and local asset references checked.
- English / Swedish toggle, animation pause/resume and newsletter iframe checked in browser.
- Newsletter iframe loads the existing Email field and Submit button. Actual signup and welcome workflow need a controlled end-to-end test before DNS cutover.
- Legacy index and December 2025 URLs have permanent redirects.
- Canonical and Open Graph URLs use yage.se; sitemap and robots are included.

## Rollback

Keep a snapshot of the current web DNS records before cutover. If the new host fails, restore only those web records and retain email records. The old GitHub Pages source and custom-domain CNAME remain in this repository.
