# AGENTS.md

catenahq/renovate-config is the shared Renovate policy preset of the catenahq org. README.md says how it is
consumed.

## Rules

- Never add `automerge: false` to a rule; propose a bench or CI check instead.
- Never pin `catenahq/**` `uses:` refs to a digest.
- Keep the `.github/catenahq.lock` pins moving: digest updates on, no release-age wait.
