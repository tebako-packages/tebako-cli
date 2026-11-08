# tebako-cli

The tebako CLI as a tebako payload — the self-hosting arc's self-package.

This feedstock compiles nothing. It re-packages the [tamatebako/tebako](https://github.com/tamatebako/tebako) release's CLI binary (the host's own format) into a `kind: toolkit` payload, published per platform, so the CLI is installable from the public registry and dispatches through the zero-runtime entrypoint (no runtime slot — the binary is native).

## What this proves

The tebako ecosystem packages its own toolchain. `tebako install tebako` resolves the CLI from the public registry, and the installed CLI presses and dispatches payloads — the full loop on tebako's own machinery, the self-hosting arc's acceptance.

## Layout

- `Tebakofile` — the recipe: the pinned tebako release (the CLI binary's source) + the toolchain pin (the tfs imager), every digest verified against the release's own SHA256SUMS at fetch time.
- `tools/build` — the repackage leg: download the platform's CLI binary (sha256-verified), stage `bin/tebako[.exe]`, pack with the pinned tfs CLI.
- `tools/stage` — name + embed the manifest + repack + read-back-validate, producing `dist/`.
- `manifests/payload.yaml` — the payload manifest template.
- `tpkg-registry.yaml` — the feedstock's own registry (the aggregate flows it).

## A bump

A new tebako line: `upstream.release` + `upstream.version` + the `tools.sha256` pins move to the new release (digests from the release's SHA256SUMS), and CI re-publishes the payload + the registry row.
