# NPM Publish

A GitHub Action for versioning, building, and publishing NPM packages to the NPM registry.

## Features

- Install dependencies and build packages
- Optional version management
- Customizable build commands
- Configurable Node.js version and registry URL
- Support for ignoring install scripts

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
| `node-version`   | Node.js version to use                                      | `20`                         | No       |
| `registry-url`   | NPM registry URL                                            | `https://registry.npmjs.org` | No       |
| `version`        | Version to set in package.json                              | -                            | No       |
| `build-command`  | Build script name from package.json                         | -                            | No       |
| `token`          | NPM authentication token (required unless using provenance) | -                            | No\*     |
| `provenance`     | Enable provenance statements (requires OIDC)                | `true`                       | No       |
| `ignore-scripts` | Run npm ci with --ignore-scripts                            | `true`                       | No       |

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

## Requirements

- Your package should have a valid `package.json`
- If using `build-command`, ensure the script exists in package.json
- NPM authentication token with publish permissions

## License

MIT
