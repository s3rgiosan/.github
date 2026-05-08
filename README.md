# .github

Shared GitHub configuration for [@s3rgiosan](https://github.com/s3rgiosan) repositories.

## Workflows

### `wp-plugin-release.yml`

Packages a WordPress plugin and attaches the resulting ZIP to a GitHub Release.

Two modes, controlled by the `build` input:

- **`build: false`** (default) — uses `git archive` against the pushed tag. Repo must commit `build/` and `vendor/` ahead of tagging. Honors every `export-ignore` directive in the plugin's `.gitattributes`.
- **`build: true`** — extracts the tagged tree via `git archive`, then runs `composer install --no-dev` and (when a `package.json` is present) `npm ci && npm run build` inside it before zipping. Excludes are still sourced from `.gitattributes`.

#### Inputs

| Name | Required | Default | Description |
| --- | --- | --- | --- |
| `slug` | yes | — | Plugin slug. Used as the inner folder name and ZIP filename prefix. |
| `build` | no | `false` | Build assets in CI before zipping. |
| `php-version` | no | `8.1` | PHP version used when `build: true`. |
| `node-version` | no | `lts/*` | Node version used when `build: true` and a `package.json` is present. |
| `generate-release-notes` | no | `true` | Auto-generate release notes from commits. |

#### Example caller

```yaml
name: Release

on:
  push:
    tags: ['*.*.*']

jobs:
  release:
    permissions:
      contents: write
    uses: s3rgiosan/.github/.github/workflows/wp-plugin-release.yml@main
    with:
      slug: my-plugin
```

## License

MIT.
