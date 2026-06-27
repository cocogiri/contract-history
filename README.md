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

## Automation

A GitHub Actions workflow in `cocogiri-meta` runs daily and on every
`contract-released` repository dispatch event. It fetches the latest tags from
each repository listed in a contract entry and opens a pull request in this
repo when the mapping changes.

### Required setup

Each contract-owning repository has a `.github/workflows/notify-contract-release.yml`
workflow that dispatches a `contract-released` event to `cocogiri-meta`. Because
cross-repository dispatch requires elevated permissions, each repository must
set a repository secret named:

```text
CROSS_REPO_DISPATCH_TOKEN
```

This token needs `repo` scope (or `public_repo` for public repositories) and
must be authorized to write to `cocogiri/cocogiri-meta`. If the secret is not
set, the workflow falls back to the default `GITHUB_TOKEN`, which typically
cannot dispatch across repositories.

### Manual fallback

If a repo has not yet been tagged for a new contract version, update the commit
SHA in `contracts.yml` manually and open a PR.
