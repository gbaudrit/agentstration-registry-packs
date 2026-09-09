# Agentstration Pack Registry

This repository publishes Packs through an Agentstration `SourceVersion`.
Its `stable` Channel is backed by the public Git repository and exposes the
Pack catalog in `catalogs/packs.yaml`.

## Test Source

Import the Source manifest from:

```text
https://raw.githubusercontent.com/gbaudrit/agentstration-registry-packs/main/source.yaml
```

Then bind `git-distribution` to a local Git Source Provider and refresh the
`stable` Channel. The `say-hello` Pack can be previewed and installed from the
discovered Pack catalog.

During preview, select:

- one local Model Profile for `hello-model`;
- one local Runtime Profile for `hello-runtime`.

The Pack installs one workspace-owned Agent. The published archive is
`artifacts/say-hello-1.0.0.zip`. This repository contains distribution
artifacts only; editable Pack sources are maintained separately.
