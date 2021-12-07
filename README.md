# Build container with JIB and publish it to GitHub Packages 
GitHub action to build container with jib and publish it to GitHub Packages.

## Requirements
- Your project need to use Maven

## Usage

The workflow, usually declared in `.github/workflows/jib-publish.yml`, looks like:
```YAML
name: JIB Container publish to GitHub Package

on:
  release:
    types: [created]

env:
  # Use docker.io for Docker Hub if empty
  REGISTRY: ghcr.io
  # github.repository as <account>/<repo>
  IMAGE_NAME: ${{ github.repository }}

jobs:
  publish:

    runs-on: ubuntu-latest
    
    - name: Set up JDK 11
      uses: actions/setup-java@v1
      with:
        java-version: 11

    - name: Buil JIB container and publish to GitHub Packages
      run: mvn compile jib:build \
      -Djib.to.image=${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:latest \
      -Djib.to.auth.username=${{ github.actor }} \
      -Djib.to.auth.password=${{ secrets.GITHUB_TOKEN }}
```