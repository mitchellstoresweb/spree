# Mitchells Fork Notes

This is the Mitchells fork of Spree, pinned at **v4.8.3** (the last BSD-3-Clause
release line; Spree 5.0+ moved to AGPL/commercial dual licensing). The default
branch is `mitchells/main`, which started at the `v4.8.3` tag — verified
byte-for-byte against the published 4.8.3 gems.

## Rules of the road

- **Never merge or cherry-pick from Spree 5.x** (or upstream `main`). That code is
  AGPL-licensed; bringing it into this BSD fork is not an option.
- Cherry-picking from upstream's `4-x-stable` branch (still BSD) is fine while
  upstream maintains it — fetch `spree/spree` locally and cherry-pick onto
  `mitchells/main`.
- Prefix fork-only commits with `MITCHELLS:` and mark divergence points in code
  with `# MITCHELLS_OVERRIDE:` comments so they're findable.

## Where the record of changes lives

`mitchells/main` begins at the `v4.8.3` tag, so the complete divergence from
upstream is simply:

```
git log --oneline v4.8.3..mitchells/main
```

Fork-only commits carry the `MITCHELLS:` prefix; anything without it is an
upstream cherry-pick. Rationale for each change lives in its merged PR.

## How the app consumes this fork

`ecommerce-web` points at this repo via `Gemfile` git sources (branch
`mitchells/main`), SHA-pinned through `Gemfile.lock` — no release tags. The
monorepo supplies `spree`, `spree_core`, `spree_api`, `spree_emails`, and
`spree_cli`; updates land via `bundle update spree` plus a lockfile commit.
