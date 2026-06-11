# Mitchells Changes

This is the Mitchells fork of Spree, pinned at **v4.8.3** (the last BSD-3-Clause
release line; Spree 5.0+ moved to AGPL/commercial dual licensing). The default
branch is `mitchells/main`, which started at the `v4.8.3` tag — verified
byte-for-byte against the published 4.8.3 gems.

Rules of the road:

- **Never merge or cherry-pick from Spree 5.x** (or upstream `main`). That code is
  AGPL-licensed; bringing it into this BSD fork is not an option.
- Cherry-picking from upstream's `4-x-stable` branch (still BSD) is fine while
  upstream maintains it — fetch `spree/spree` locally and cherry-pick onto
  `mitchells/main`.
- Prefix fork-only commits with `MITCHELLS:` and mark divergence points in code
  with `# MITCHELLS_OVERRIDE:` comments so diffs against upstream stay findable.
- Record every divergence in this file.

Consumed by the `ecommerce-web` app via `Gemfile` git sources (branch
`mitchells/main`, SHA-pinned through `Gemfile.lock` — no release tags).

## Changes vs. upstream v4.8.3

### Stripped Spree 5.0 deprecation warnings (2026-06)

Spree 4.8 warns about removals planned for Spree 5.0, which this fork will never
run. The warnings that fire on hot paths drowned out real (Rails) deprecations in
the app's test runs. Removed the `Spree::Deprecation.warn` calls — behavior is
unchanged — from:

- `core/app/models/spree/product_property.rb` — `ProductProperty#property_name=`
  (hit by the admin product form via nested attributes)
- `core/lib/spree/core/controller_helpers/common.rb` — `title`, `default_title`,
  `accurate_title`, `set_user_language`, `get_layout` (the module is included in
  `Spree::BaseController` and `layout :get_layout` runs on every render)

Other `Spree::Deprecation.warn` sites in core (14 files) are untouched — they sit
on code paths the app never executes. Strip them the same way if they start firing.
