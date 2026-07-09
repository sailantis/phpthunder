# Release Versioning

PhpThunder uses a **two-version system**: a human-readable **version tag** and a **VS Marketplace version**. The mapping between them is deterministic so that multiple prerelease iterations of the same base version can coexist on the Marketplace.

## Why two versions?

The VS Code Marketplace only accepts plain semver (`major.minor.patch`) and treats each unique version as a distinct extension entry. It also requires that prerelease versions be marked with the `--pre-release` flag rather than embedded in the version string.

This means we cannot use version tags like `2033.1.0-beta.1` and `2033.1.0-beta.2` directly as Marketplace versions — they would either be rejected or overwrite each other. We map every version tag to a unique marketplace version instead.

## Channels

PhpThunder supports four release channels:

- **stable** — production releases
- **rc** — release candidates
- **beta** — public previews
- **alpha** — early testing

## Version Mapping

The Marketplace version is computed from the version tag using a fixed scheme:

```
marketplacePatch = channelBase + (basePatch * 100) + index
```

| Component    | Value                                                     |
| ------------ | --------------------------------------------------------- |
| `channelBase`| `alpha=10000`, `beta=30000`, `rc=50000`, `stable=90000`  |
| `basePatch`  | The patch component of the tag's base version             |
| `index`      | Prerelease iteration number, `0` for stable               |

The Marketplace version keeps the original `major.minor` of the tag; only the patch is recalculated.

## Examples

| Version Tag            | Channel | Marketplace Version |
| ---------------------- | ------- | ------------------- |
| `2033.1.0-alpha.1`    | alpha   | `2033.1.10001`      |
| `2033.1.1-alpha.1`    | alpha   | `2033.1.10101`      |
| `2033.1.1-beta.1`     | beta    | `2033.1.30101`      |
| `2033.1.1-beta.2`     | beta    | `2033.1.30102`      |
| `2033.1.1-rc.1`       | rc      | `2033.1.50101`      |
| `2033.1.1`            | stable  | `2033.1.90100`      |
| `2033.1.2-alpha.1`    | alpha   | `2033.1.10201`      |
| `2033.1.2-beta.1`     | beta    | `2033.1.30201`      |
| `2033.1.2`            | stable  | `2033.1.90200`      |
| `2033.1.15-beta.1`    | beta    | `2033.1.31501`      |
| `2033.1.15`           | stable  | `2033.1.91500`      |
