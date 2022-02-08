# Build container with JIB and publish it to GitHub Packages

GitHub action to build container with jib and publish it to GitHub Packages.

## Requirements

- Your project need to use Maven

## Usage

The workflow, usually declared in `.github/workflows/jib-publish.yml`, looks like:

```YAML
name: JIB container publish

on:
  release:
    types: [created]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - name: JIB container build and publish
        uses: MathieuSoysal/jib-container-publish.yml@v2.0.2
        with:
          # Use docker.io for Docker Hub if empty
          REGISTRY: gcr.io
          # github.repository as <your-account>/<your-repo>
          IMAGE_NAME: ${{ github.repository }}
          # Tag name of the image to publish
          tag-name: ${{ github.event.release.tag_name }}
          # Username to login to registry
          USERNAME: ${{ github.actor }}
          # Password to login to registry
          PASSWORD: ${{ secrets.GITHUB_TOKEN }}
          java-version: 17

```

### You need to choice a registry other than GitHub Package ?

You can change the `REGISTRY`,`USERNAME`,`PASSWORD` to publish in the registry of your choice:

```YAML
  # Use docker.io for Docker Hub if empty
  REGISTRY:
  # github.repository as <account>/<repo>
  IMAGE_NAME: ${{ github.repository }}
  # Username to login to registry
  USERNAME:
  # Password to login to registry
  PASSWORD:
```

## License

The Dockerfile and associated scripts and documentation in this project are released under the [GPL-3.0 License](https://github.com/MathieuSoysal/jib-container-publish.yml/blob/main/LICENSE).
