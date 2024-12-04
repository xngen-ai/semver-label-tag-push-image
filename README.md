# Semver Label Tag Push Image Action

This GitHub Action combines several common Docker image management tasks into a single action:

- Semantic versioning
- Image labeling with OCI annotations
- Image tagging
- Registry authentication and pushing

## Usage

```yaml
- uses: xngen-ai/semver-label-tag-push-image@v1
  with:
    registry: 'ghcr.io'
    org_name: 'your-org'
    image_name: 'your-image'
    image_title: 'Your Image Title'
    image_description: 'Your Image Description'
    version_increment: 'patch'  # or 'minor' or 'major'
    registry_password: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `registry` | Container registry to push to | Yes | 'ghcr.io' |
| `org_name` | Organization name for the image | Yes | - |
| `image_name` | Name of the image | Yes | - |
| `image_title` | Title of the image for OCI labels | Yes | - |
| `image_description` | Description of the image for OCI labels | Yes | - |
| `version_increment` | Version increment type (major, minor, patch) | Yes | 'patch' |
| `release_branch` | Branch to use for versioning | No | 'main' |
| `registry_username` | Username for registry authentication | No | ${{ github.actor }} |
| `registry_password` | Password for registry authentication | Yes | - |

## Example Workflow

```yaml
name: Build and Push Docker Image

on:
  push:
    branches: [ main ]
  workflow_dispatch:
    inputs:
      version_increment:
        description: 'Version increment type'
        required: true
        default: 'patch'
        type: choice
        options:
          - patch
          - minor
          - major

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    
    steps:
      - uses: xngen-ai/semver-label-tag-push-image@v1
        with:
          registry: 'ghcr.io'
          org_name: ${{ github.repository_owner }}
          image_name: 'my-image'
          image_title: 'My Docker Image'
          image_description: 'Description of my Docker image'
          version_increment: ${{ github.event.inputs.version_increment }}
          registry_password: ${{ secrets.GITHUB_TOKEN }}
```

## Requirements

- A Dockerfile must exist in the root of your repository
- The repository must have proper permissions set up for the registry (e.g., `packages: write` for GitHub Packages)
- Git history must be available (the action uses `fetch-depth: 0`)

## License

MIT License - see the [LICENSE](LICENSE) file for details
