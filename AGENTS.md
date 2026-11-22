# AGENTS.md

catenahq/renovate-config is the shared Renovate policy preset of the catenahq org. README.md says how it is
consumed.

## Rules

- Never add `automerge: false` to a rule; propose a bench or CI check instead.
- Pin `catenahq/**` `uses:` refs to a digest (`@<sha> # <branch>`) and keep them moving by Renovate, one group per
  consuming repository.
- Keep `platformAutomerge` on only while every repository has the `ci / gate` branch ruleset.
- Keep the `.github/catenahq.lock` pins moving: digest updates on, no release-age wait.
