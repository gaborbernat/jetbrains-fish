# Contributing

## Development

- Run `mise install` to get the JDK pinned in `mise.toml` and checksummed in `mise.lock`.
- Run `./gradlew buildPlugin` to build the plugin.
- Run `./gradlew test` to run the tests.
- Run `./gradlew runIde` to run the IDE with the plugin installed.

## Code Style

Run `./gradlew ktlintFormat` before committing to ensure consistent formatting.

## News fragments

For a user-visible change, add `changelog.d/+short-name.<category>.md`. Use `feature` for a new capability, `change` for
changed behavior, `bugfix` for a fix, and `doc` for user documentation. `security`, `performance`, `deprecation`,
`removal`, `breaking`, and `packaging` describe other user-visible results. Give separate changes separate files. The
release job adds the squash PR link and human author credit after merge. Apply `skip news` to maintenance PRs; bot PRs
without fragments receive that label automatically.

## Releasing

Run **Prepare release** on `main` with `auto` after merging the intended fragments. A `breaking` fragment selects a
major version, a `feature` fragment selects a minor version, and other categories select a patch version. Towncrier
updates `CHANGELOG.md`; `prek` checks the result before the repository-scoped App pushes the release commit and
`v`-prefixed tag. The dispatch does not ask for a version number.

The tag workflow checks the notes, sets `pluginVersion` for that build, then builds and verifies a signed plugin ZIP. A
keyless job compares its ZIP members with the
[JetBrains Marketplace](https://plugins.jetbrains.com/plugin/29973-fish-shell) version. If absent, a fresh job uploads
the signed ZIP with the Marketplace token. Another keyless job checks the published members before GitHub attaches the
original ZIP, verified by SHA256, to its Release. The workflow then opens a PR for the next development version. The App
key is held by `release-auth`; signing and Marketplace credentials are held by the tag-restricted `release` environment.
