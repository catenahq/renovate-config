# catenahq/renovate-config

The shared Renovate policy preset for the catenahq org. Extend it
from any repo's `renovate.json`:

```json
{ "extends": ["github>catenahq/renovate-config"] }
```

It layers Catena operational config (PR concurrency / hourly limits, timezone,
labels, daily lockfile maintenance, low-risk automerge groups) on top of the
generic security baseline `github>catenahq/scanctl:secure-base` (7-day adoption
cooldown, GitHub Actions SHA-pinning, CVE fast-track).

The sibling pins in `.github/catenahq.lock` (lines `catenahq/<repo> <40-hex sha> #
<branch>`) move through pull requests, with no release-age wait and one group per
source repository. The first-party `uses:` refs of `catenahq/**` (`@<sha> # <branch>`) move
the same way, all together in one pull request per consuming repository.
Digest updates of `marcusquinn/aidevops` are on.

`platformAutomerge` is on: Renovate enables GitHub auto-merge on its pull requests and the
`ci / gate` branch ruleset of the repository decides when the merge happens, so a repository
needs that ruleset before it extends this preset.

In `ci/run`, a `# renovate: datasource=... depName=...` comment above a `*_VERSION=` line, or
above an `*_IMAGE=<image>:<tag>@sha256:<digest>` line (datasource `docker`), is tracked too.

Public on purpose: it carries no secrets, so public and private consumers alike
resolve it cleanly and the policy stays transparent. The self-hosted Renovate
**runner** lives separately in the private `catenahq/renovate` repo, which
self-consumes this preset; run timing (every 4 hours) is owned by that runner's
cron.

Pull requests run the shared PR workflow of catenahq/contracts (`.github/workflows/ci.yml`,
checks in `.github/checks.yml`). Its `presets` check, `bash ci/run presets`, runs Renovate's
own `renovate-config-validator --strict` (renovate version pinned in `ci/run`) on every preset
file in the repo, currently `default.json`. It fails on unknown options, mistyped values and
invalid `packageRules`. A new preset file must be added to the command in `ci/run`.

Resolution chain:

```
scanctl:secure-base   (public, generic security baseline)
        ^ extends
renovate-config        (this repo: Catena org policy)
        ^ extends
every catenahq repo's renovate.json
```
