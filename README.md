# immich-cli

[![Build Status](https://img.shields.io/github/actions/workflow/status/daemonless/immich-cli/build.yaml?style=flat-square&label=Build&color=green)](https://github.com/daemonless/immich-cli/actions)
[![Last Commit](https://img.shields.io/github/last-commit/daemonless/immich-cli?style=flat-square&label=Last+Commit&color=blue)](https://github.com/daemonless/immich-cli/commits)

A FreeBSD daemonless image for the official [Immich](https://github.com/immich-app/immich) command-line client (`@immich/cli`) — `upload`, `import`, `server-info` etc.

| | |
|---|---|
| **Registry** | `ghcr.io/daemonless/immich-cli` |
| **Upstream** | [`immich-app/immich`](https://github.com/immich-app/immich) (`packages/cli`, npm [`@immich/cli`](https://www.npmjs.com/package/@immich/cli)) |
| **License** | App: AGPL-3.0 · Packaging: BSD-2-Clause |

## Why this is needed 

`@immich/cli` is an official tool that is useful for running bulk image imports. It is pure JavaScript so it runs on native FreeBSD node, and complements the rest of the daemonless images for Immich.

## Usage

This is a **one-shot tool**, not a long-running service: the entrypoint is `immich`, runs your command to completion, and exits. Run it with `--rm` and pass the command as arguments.

```bash
podman run --rm --network host \
  -v /path/to/photos:/import:ro \
  -e IMMICH_INSTANCE_URL=http://localhost:2283/api \
  -e IMMICH_API_KEY="$IMMICH_API_KEY" \
  ghcr.io/daemonless/immich-cli \
  upload --recursive --album-name "My Photos" /import
```

- Media to upload is mounted (read-only) at **`/import`**, which is also the working directory.
- The server URL and API key are supplied at run time via `IMMICH_INSTANCE_URL` and `IMMICH_API_KEY` (or run `immich login` interactively).
- Add `--dry-run` to preview before uploading.

Check the version:

```bash
podman run --rm ghcr.io/daemonless/immich-cli --version
```

**Architectures:** amd64
**User:** `bsd` (unprivileged)
**Base:** FreeBSD 15.0 (`ghcr.io/daemonless/base-core`)
