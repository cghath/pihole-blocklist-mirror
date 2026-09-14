# pihole-blocklist-mirror

A private, daily-synced copy of the blocklists my Pi-hole (`arkwatcher`) subscribes to.
Not a git mirror of the source repos (no upstream history) - just their current content,
re-fetched and committed once a day by `.github/workflows/sync.yml` so there's a stable,
owned copy independent of the source going down or rate-limiting a fetch.

## Sources

| File | Source |
|---|---|
| `lists/stevenblack-hosts.txt` | https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts |
| `lists/perflyst-smarttv.txt` | https://perflyst.github.io/PiHoleBlocklist/SmartTV.txt |
| `lists/hagezi-pro.txt` | https://raw.githubusercontent.com/hagezi/dns-blocklists/main/domains/pro.txt |

## How the sync works

Runs daily at 06:00 UTC (`workflow_dispatch` also lets it be triggered manually from the
Actions tab). Each fetch is defensive: if a source is unreachable or rate-limited (this
has genuinely happened with HaGeZi's `pro.txt` on `raw.githubusercontent.com`), that
file is left as-is rather than being overwritten with an error page - the last known-good
copy stays committed until a fetch actually succeeds.

## Note on this being private

Since this repo is private, its files aren't fetchable via a plain
`raw.githubusercontent.com` URL the way Pi-hole's own adlist fetcher expects - that only
works for public repos (or with an auth token Pi-hole's simple URL-based fetcher doesn't
support). This repo exists as a backup/archive for now, not something Pi-hole points at
directly. If that changes, either this repo needs to go public, or a different serving
mechanism (a small authenticated proxy, GitHub Pages on a public repo, etc.) is needed.
