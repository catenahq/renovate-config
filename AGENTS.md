# AGENTS.md

catenahq/renovate-config is the shared Renovate policy preset of the catenahq org. README.md says how it is
consumed.

## Rules

- Never add `automerge: false` to a rule; propose a bench or CI check instead.
- Keep `catenahq/**` refs pinned to a digest with digest updates on and no release-age wait.
