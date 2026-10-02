# khinse-labs/workflows

Shared GitHub Actions for khinse-labs repositories. Fix CI setup once here and every repository that uses it picks it up.

## Set up Node and pnpm

A composite action to call from your own CI and deploy jobs, before your repository's own steps. Installs pnpm from `packageManager` in `package.json`, Node from `.node-version`, restores the pnpm cache and runs `pnpm install --frozen-lockfile`. Check out first.

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
