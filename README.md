# Fluent Bit: ReadyStack build kit

The offline build kit for the Docker image `readystack/fluentbit:v5.2.0-CE-ubuntu24.04-r2`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v5.2.0-CE-ubuntu24.04-r2](https://github.com/readystack-dev/fluentbit/releases/tag/v5.2.0-CE-ubuntu24.04-r2)
- Image: [`docker.io/readystack/fluentbit:v5.2.0-CE-ubuntu24.04-r2`](https://hub.docker.com/r/readystack/fluentbit/tags), manifest digest `sha256:e968022ca2fa8d2f6ded3e00b7338a57fe598425b2da5f382f23b19d5ecd90f7`
- Product page: https://readystack.dev/products/fluentbit
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=fluentbit

Fluent Bit is distributed under its upstream licence (Apache-2.0); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [v5.2.0-CE-ubuntu24.04-r2 release](https://github.com/readystack-dev/fluentbit/releases/tag/v5.2.0-CE-ubuntu24.04-r2): `ReadyStack-fluentbit-v5.2.0-CE-ubuntu24.04-r2.tar.gz` (642.3 MB), SHA-256 `1505a7207dee959e9933a413107bdc61b61ace448665c5115ae4e04ad14b28bc`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-fluentbit-v5.2.0-CE-ubuntu24.04-r2.rebuild.json.sigstore ReadyStack-fluentbit-v5.2.0-CE-ubuntu24.04-r2.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-fluentbit-v5.2.0-CE-ubuntu24.04-r2.tar.gz
cd v5.2.0-CE-ubuntu24.04-r2
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-fluentbit-v5.2.0-CE-ubuntu24.04-r2.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/fluentbit/recipe. The complete kit is the release download. Each release is a tag here.
