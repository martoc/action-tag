# Tag Repository Action

This GitHub Action tags a repository using semantic versioning based on the commit message. It supports tagging with different formats and provides options for skipping tag creation or loading previously created tags.

## Inputs

| Name       | Description                                | Required | Default |
|------------|--------------------------------------------|----------|---------|
| skip-push  | Skip tag creation for pull requests.       | false    | false   |
| execute    | The action to perform: `tag` or `load`.    | false    | tag     |
| format     | The format of the tag (`semver` or `PEP440`). | false | semver  |

## Outputs

| Name    | Description         |
|---------|---------------------|
| version | The tagged version. |

## Steps

### 1. Tag Repository
- Fetches existing tags using `git fetch --tags`.
- **If the event is a pull request:**
  - Skips tag creation and calculates a release candidate tag.
  - Uses the PEP440 format or a short SHA for the tag version.
- **Otherwise:**
  - Calculates the next semantic version using the `martoc/semver:1.5.5` Docker image.
  - Creates floating tags for major and minor versions.
  - Pushes tags unless `skip-push` is set to `true`.

### 2. Upload Tags
- Uploads the generated tags as an artifact if `execute` is set to `tag`.

### 3. Download Tags
- Downloads previously created tags as an artifact if `execute` is set to `load`.

### 4. Load Tags into Environment
- Loads the downloaded tags into the GitHub Actions environment if `execute` is set to `load`.

## Example Usagename: 

```yaml
on:
  push:
    branches:
      - main

jobs:
  tag:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Tag repository
        uses: martoc/action-tag@v1
        with:
          skip-push: false
          execute: tag
          format: semver
```
## Notes

The action uses the `GITHUB_RUN_ID` to append a unique build ID to the tag when using the `PEP440` format or for release candidate tags.
Floating tags (e.g., v1, v1.2) are created for major and minor versions for easier reference.
