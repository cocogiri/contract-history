# Cocogiri Contract History

This repository maintains the canonical mapping between contract versions and
the concrete repository tags/commits that implement them.

## Why this exists

Multiple repositories consume the same cross-boundary contract (notably the
COG ↔ SDL capability bridge contract). Rather than hunting through each repo's
history, external consumers and release tooling can read this single file to
discover which tags correspond to a given contract version.

## Usage

See `contracts.yml` for the current mapping. Each entry records:

- `version`: the semantic contract version
- `date`: ISO 8601 date when the version was ratified
- `repositories`: map of repo name → tag or commit SHA

## Adding a new contract version

1. Finalize the contract changes in the canonical owner repo.
2. Tag each consuming/producing repo with a release that supports the new
   contract version.
3. Append a new entry to `contracts.yml`.
4. Open a PR against this repo.
