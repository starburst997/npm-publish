# NPM Publish

A GitHub Action for versioning, building, and publishing NPM packages to the NPM registry.

## Features

- Install dependencies and build packages
- Auto-detects package manager (npm, pnpm, yarn)
- Optional version management
- Customizable build commands
- Configurable Node.js version and registry URL
- Support for ignoring install scripts
- OIDC trusted publishing support

## Usage

```yaml
- name: Publish to NPM
  uses: starburst997/npm-publish@v1
  with:
    token: ${{ secrets.NPM_TOKEN }}
```

## Inputs

| Input            | Description                                                 | Default                      | Required |
| ---------------- | ----------------------------------------------------------- | ---------------------------- | -------- |
| `node-version`   | Node.js version to use                                      | `24`                         | No       |
| `registry-url`   | NPM registry URL                                            | `https://registry.npmjs.org` | No       |
| `version`        | Version to set in package.json                              | -                            | No       |
| `build-command`  | Build script name from package.json                         | -                            | No       |
| `token`          | NPM authentication token (required unless using provenance) | -                            | No\*     |
| `provenance`     | Enable provenance statements (requires OIDC)                | `true`                       | No       |
| `ignore-scripts` | Run install with --ignore-scripts                           | `true`                       | No       |
| `pnpm-version`   | pnpm version (auto-detected from packageManager field)      | `10`                         | No       |

## Examples

### Basic publish

```yaml
- uses: starburst997/npm-publish@v1
  with:
    token: ${{ secrets.NPM_TOKEN }}
```

### With version update

```yaml
- uses: starburst997/npm-publish@v1
  with:
    version: "1.2.3"
    token: ${{ secrets.NPM_TOKEN }}
```

### With build step

```yaml
- uses: starburst997/npm-publish@v1
  with:
    build-command: "build"
    token: ${{ secrets.NPM_TOKEN }}
```

### Custom Node.js version

```yaml
- uses: starburst997/npm-publish@v1
  with:
    node-version: "18"
    token: ${{ secrets.NPM_TOKEN }}
```

### With provenance (using OIDC)

```yaml
- uses: starburst997/npm-publish@v1
  with:
    provenance: true

  # Need permissions at the top of files as well
  permissions:
    id-token: write
    contents: read
```

### With pnpm

The action automatically detects pnpm from `pnpm-lock.yaml` and reads the version from your `package.json` `packageManager` field:

```json
{
  "packageManager": "pnpm@10.12.1"
}
```

Or specify the version manually:

```yaml
- uses: starburst997/npm-publish@v1
  with:
    pnpm-version: "10.12.1"
    token: ${{ secrets.NPM_TOKEN }}
```

## Package Manager Support

The action automatically detects your package manager based on lockfiles:

- **pnpm**: Detected via `pnpm-lock.yaml`
  - Auto-detects version from `packageManager` field in package.json
  - Falls back to version 10 if not specified
- **yarn**: Detected via `yarn.lock`
- **npm**: Detected via `package-lock.json` or used as default

## Requirements

- Your package should have a valid `package.json`
- If using `build-command`, ensure the script exists in package.json
- NPM authentication token with publish permissions (or OIDC setup for provenance)

## License

MIT
