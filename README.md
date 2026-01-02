[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# action-tag

A GitHub Action that calculates and applies semantic version tags to repositories based on commit messages. It follows the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification to determine version increments and supports multiple tag formats including SemVer and PEP440.

## Features

- Automatic semantic version calculation from commit messages
- Floating tags for major and minor versions (e.g., `v1`, `v1.2`)
- Support for SemVer and PEP440 formats
- Release candidate tags for pull requests
- Tag persistence across workflow jobs via artifacts

## Quick Start

```yaml
- name: Checkout
  uses: actions/checkout@v4
  with:
    fetch-depth: 50
    fetch-tags: true

- name: Tag
  uses: martoc/action-tag@v0
```

## Documentation

- [Usage Guide](./docs/USAGE.md) - Detailed usage instructions and examples
- [Code Style](./docs/CODESTYLE.md) - Code style guidelines for contributors

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `skip-push` | Skip pushing tags to remote | No | `false` |
| `execute` | Action to perform: `tag` or `load` | No | `tag` |
| `format` | Tag format: `semver` or `PEP440` | No | `semver` |

## Outputs

| Output | Description |
|--------|-------------|
| `version` | The calculated version |
| `TAG_VERSION` | Full version string |
| `TAG_NAME` | Tag name with `v` prefix |
| `TAG_FLOATING_MAJOR_NAME` | Floating major version tag |
| `TAG_FLOATING_MINOR_NAME` | Floating minor version tag |

## Environment Variables

The following environment variables are set after execution:

- `TAG_VERSION` - The calculated version
- `TAG_NAME` - Tag name with `v` prefix
- `TAG_SHA` - Git SHA of the tagged commit

## Licence

This project is licenced under the MIT Licence - see the [LICENCE](LICENSE) file for details.
