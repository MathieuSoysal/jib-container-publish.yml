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

env:
  # Use docker.io for Docker Hub if empty
  REGISTRY: ghcr.io
  # github.repository as <account>/<repo>
  IMAGE_NAME: ${{ github.repository }}
  # Username to login to registry
  USERNAME: ${{ github.actor }}
  # Password to login to registry
  PASSWORD: ${{ secrets.GITHUB_TOKEN }}

jobs:
  publish:

    runs-on: ubuntu-latest
    
    - name: Set up JDK 11
      uses: actions/setup-java@v1
      with:
        java-version: 11

    - name: Buil JIB container and publish to GitHub Packages
      run: mvn compile com.google.cloud.tools:jib-maven-plugin:3.1.4:build \
      -Djib.to.image=${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:latest \
      -Djib.to.auth.username=${{ env.USERNAME }} \
      -Djib.to.auth.password=${{ env.PASSWORD }}
```
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

If your Java project is not in Java 11, don't forget to modify these two lines:
```YAML
    - name: Set up JDK 11
      uses: actions/setup-java@v1
      with:
        java-version: 11
```
## License
The Dockerfile and associated scripts and documentation in this project are released under the GPL-3.0 License.
