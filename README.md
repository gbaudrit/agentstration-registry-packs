# Agentstration Pack Source

This repository distributes reusable Packs through an Agentstration
`SourceVersion`. Source discovery and Channel content transport remain separate:

- the official static registry may publish an exact copy and digest of a
  versioned manifest from `source-versions/`;
- the `git-distribution` Source Provider fetches the compatible maintenance
  branch declared by that manifest;
- Pack archives remain descendants of their `PackCatalog` and are fetched only
  through the materialized Channel Snapshot.

GitHub Pages is not enabled for this repository. The host-neutral official
registry is published separately by
[`gbaudrit/agentstration-registry`](https://github.com/gbaudrit/agentstration-registry).

## Published Source versions

| Agentstration line | Compatibility interval | SourceVersion | Channel ref | Exact manifest |
| --- | --- | --- | --- | --- |
| `0.2.x` | `>= 0.2.0-alpha.1`, `< 0.3.0` | `1` | `refs/heads/release/0.2` | `source-versions/1/source.yaml` |

`source.yaml` is a convenience copy of the current manifest for direct testing.
Registry publication and reproducible imports must use the immutable path under
`source-versions/`. Once published in a registry, a versioned manifest must not
be rewritten.

## Test the 0.2 Source

Import the exact Source manifest from:

```text
https://raw.githubusercontent.com/gbaudrit/agentstration-registry-packs/main/source-versions/1/source.yaml
```

Then bind `git-distribution` to a local Git Source Provider and refresh the
`stable` Channel. The provider resolves `refs/heads/release/0.2`, and the
resulting immutable Snapshot exposes the Pack catalog in `catalogs/packs.yaml`.

The `say-hello` Pack can then be previewed and installed from the discovered
catalog. During preview, select:

- one local Model Profile for `hello-model`;
- one local Runtime Profile for `hello-runtime`.

The Pack installs one workspace-owned Agent. Its published archive is
`catalogs/artifacts/say-hello-1.0.0.zip`. Catalog entry paths are resolved
relative to their catalog file. This repository contains distribution
artifacts only; editable Pack sources are maintained separately.

## Add a new Agentstration release line

Agentstration compatibility remains authoritative on each Source Channel; the
branch name and registry shard are only publication and discovery mechanisms.
For a new incompatible Agentstration line:

1. Create a `release/<major>.<minor>` maintenance branch containing only the
   Packs and catalogs supported by that line.
2. Create a new, never-reused opaque `SourceVersion` value. Do not edit an
   already published manifest.
3. Bound the Channel compatibility interval to the release line and set its Git
   `ref` to that maintenance branch.
4. Store the exact manifest at `source-versions/<source-version>/source.yaml`;
   keep `source.yaml` only as the mutable direct-test convenience copy.
5. Add that exact manifest and digest to every compatible shard in the official
   registry. Registry `latest` remains shard-local and never replaces the exact
   SourceVersion and digest selected for import.

A SourceVersion may appear in more than one compatible registry shard only when
its identity, manifest URL, and digest are identical. Agentstration still
revalidates Channel compatibility after import. The registry never transports
the Pack archive or other Channel content.
