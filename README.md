# khinse-labs/workflows

Shared GitHub Actions for khinse-labs repositories. Fix CI setup once here and every repository that uses it picks it up.

## Set up Node and pnpm

A composite action to call from your own CI and deploy jobs, before your repository's own steps. Installs pnpm from `packageManager` in `package.json`, Node from `.node-version`, restores the pnpm cache and runs `pnpm install --frozen-lockfile`. Check out first.

```yaml
steps:
  - uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1 # v7.0.1
    with:
      persist-credentials: false
  - uses: khinse-labs/workflows/actions/setup-node-pnpm@4134aa05f5ac843d4037a77890f1b3a7e91037df # v1.2.0
  - run: pnpm build
```

Set `install: "false"` to skip the install.

## Release

A reusable workflow that cuts the next version from Conventional Commits with [git-cliff](https://git-cliff.org): it picks the version (`feat` bumps the minor version, `fix` the patch, a breaking change the major), writes the notes with the shared `cliff.toml` in [khinse-labs/.github](https://github.com/khinse-labs/.github), and publishes a GitHub release with its tag. When every unreleased commit is tooling or process (`chore`, `ci`, `build`, `style`, `test`, `spike`) and none is breaking, it releases nothing.

Call it from a manual workflow, so a release stays a deliberate click:

```yaml
on:
  workflow_dispatch:

jobs:
  release:
    uses: khinse-labs/workflows/.github/workflows/release.yml@4134aa05f5ac843d4037a77890f1b3a7e91037df # v1.2.0
    permissions:
      contents: write
```

Inputs: `bump` (`auto`, `major`, `minor` or `patch`), `major-tag` (also move the major tag, for actions and reusable workflows like the ones here) and `config-ref` (the `khinse-labs/.github` tag to read `cliff.toml` from).

Releases made with the job's `GITHUB_TOKEN` do not start other workflows: a deploy that runs on tag pushes needs a GitHub App token.

## Versions

khinse-labs requires every action to be pinned to a full commit SHA, so call a release by its commit with the version in a comment, as above. Each release is a `vX.Y.Z` tag, and the major tag (`v1`) follows the latest compatible release; breaking changes get `v2`. This repository releases itself with [release-self.yml](.github/workflows/release-self.yml).

## Licence

[CC0 1.0](LICENSE).
