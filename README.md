# khinse-labs/workflows

Reusable GitHub Actions workflows and actions for khinse-labs repositories. Fix CI once here and every repository that calls it picks it up.

## Node CI

Checks out, sets up Node and pnpm, installs, then runs pnpm scripts in order.

```yaml
# .github/workflows/ci.yml in your repository
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    uses: khinse-labs/workflows/.github/workflows/node-ci.yml@v1
    with:
      scripts: verify build
```

| Input | Default | |
| --- | --- | --- |
| `scripts` | `lint typecheck test` | Space-separated pnpm scripts, run in order |
| `lfs` | `false` | Fetch Git LFS files on checkout |
| `node-version-file` | `.node-version` | File holding the Node version |
| `timeout-minutes` | `20` | Job timeout |

## Set up Node and pnpm

A composite action for your own jobs (deploys, releases). Installs pnpm from `packageManager` in `package.json`, Node from `.node-version`, restores the pnpm cache and runs `pnpm install --frozen-lockfile`. Check out first.

```yaml
steps:
  - uses: actions/checkout@v7
    with:
      persist-credentials: false
  - uses: khinse-labs/workflows/actions/setup-node-pnpm@v1
  - run: pnpm build
```

Set `install: "false"` to skip the install.

## Versions

Call a major tag (`@v1`). Compatible changes move the `v1` tag; breaking changes get `v2`.

## Licence

[CC0 1.0](LICENSE).
