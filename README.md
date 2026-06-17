# catenahq/renovate-config

The shared Renovate policy preset for the catenahq org. Extend it
from any repo's `renovate.json`:

```json
{ "extends": ["github>catenahq/renovate-config"] }
```

It layers Catena operational config (PR concurrency / hourly limits, timezone,
labels, weekly lockfile maintenance, low-risk automerge groups) on top of the
generic security baseline `github>catenahq/scanctl:secure-base` (7-day adoption
cooldown, GitHub Actions SHA-pinning, CVE fast-track).

Public on purpose: it carries no secrets, so public and private consumers alike
resolve it cleanly and the policy stays transparent. The self-hosted Renovate
**runner** lives separately in the private `catenahq/renovate` repo, which
self-consumes this preset; run timing (every 4 hours) is owned by that runner's
cron.

Resolution chain:

```
scanctl:secure-base   (public, generic security baseline)
        ^ extends
renovate-config        (this repo: Catena org policy)
        ^ extends
every catenahq repo's renovate.json
```
