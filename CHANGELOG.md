# Changelog

All notable changes to `cnice` are documented here. The project follows
Semantic Versioning.

## 0.1.3 - 2026-11-01

- The Rust crate ships the Typst package (`typst.toml`, `typst/**` and the
  `tables/**` its sources read) and exposes it as data in the new
  `cnice::typst` module: `NAMESPACE`, `NAME`, `VERSION`, `spec()`
  (`@preview/cnice:<version>`), and `FILES` (package-relative paths with
  their bytes, sorted by path). Rust hosts that embed Typst, such as cletter,
  register the package from the crate instead of vendoring a copy, so Rust
  and Typst come from the same release. No new dependencies.
- A Rust test checks `FILES` against the files on disk and `typst.toml`'s
  name and version against the crate; `scripts/check-versions.py` also
  requires the Typst package name and files in the crate package.

## 0.1.2 - 2026-09-24

- Drop the duplicate `LICENSES/LGPL-3.0-only WITH LGPL-3.0-linking-exception.txt`
  (identical to `LGPL-3.0-linking-exception.txt`); JSR rejects paths with spaces.
- 0.1.1 reached crates.io only; npm, JSR and PyPI go from 0.1.0 to 0.1.2.

## 0.1.1 - 2026-09-24

- Repository moved to github.com/corbet-foss/cnice; registry metadata points there.
- Released from a single tag through CI (crates.io and JSR trusted publishing).
- Add explicit Swiss French (`fr-ch`) and Swiss Italian (`it-ch`)
  salutation rows (same norms as `fr`/`it`); vectors pin both locales.

## 0.1.0 - 2026-09-18

- Replace the ported German-only matcher with the uniform salutation
  renderer: one code path for every locale in `tables/salutation.json`
  (de/de-ch/de-at/de-li, fr, it, rm, en/en-gb/en-us), driven entirely by
  table rows — no per-language branch. `Region`, `parse_region`,
  `region_uses_comma` and `de_salutation` are gone; parsers take an
  explicit locale, `salutation(locale, name)` renders through the row
  templates, `honorific_warning(location, locale, name)` replaces
  `de_honorific_warning`, and `is_supported(locale)` reports row coverage.
- Add Romansh (Rumantsch Grischun) rows; the `rm` salutation row and
  `Cordials salids` closing are marked native-pending (see Review below)
  and pin provisional bytes for determinism.
- Add `rm` closing row (`Cordials salids`, native-pending).

## Review

- `rm` salutation row and closing: provisional bytes awaiting native
  review (opening attested in Lia Rumantscha/BAKOM correspondence).
  Vectors prove determinism, not correctness.

### Initial scaffold (2026-09-17)

- New facade crate `cnice` over formulaic correspondence phrases:
  `cnice.greet` ports 100% of the `cgreet` public API (German salutations,
  regions, title handling, surname extraction, validation warnings) and
  `cnice.farewell` ports 100% of the `cfarewell` public API (closings,
  available locales) with identical names, semantics, and shared
  conformance vectors (74 greet + 57 farewell = 131 total).
- License this new release line under LGPL-3.0-only WITH LGPL-3.0-linking-exception across Cargo, npm, JSR,
  Python and Typst, with the complete LGPL and incorporated GPL notices.
- The `cgreet` and `cfarewell` repositories keep their original grants untouched; this facade does not relicense them.
