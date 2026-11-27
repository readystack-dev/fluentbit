[![ReadyStack](https://readystack.dev/logo.svg)](https://readystack.dev)

**Stable, Open-source Releases, Automated** · [readystack.dev](https://readystack.dev)

[![Docker Hub](https://img.shields.io/badge/Docker%20Hub-readystack%2Ffluentbit-2496ED?logo=docker&logoColor=white)](https://hub.docker.com/r/readystack/fluentbit) [![Docker pulls](https://img.shields.io/docker/pulls/readystack/fluentbit)](https://hub.docker.com/r/readystack/fluentbit) [![Grade at release](https://img.shields.io/badge/grade%20at%20release-A%20%2894%25%29-2ea44f)](https://readystack.dev/products/fluentbit) [![Signed: cosign](https://img.shields.io/badge/signed-cosign-2ea44f)](https://readystack.dev/keys/cosign.pub) ![SBOM: SPDX](https://img.shields.io/badge/SBOM-SPDX-2ea44f) ![Provenance: SLSA v1](https://img.shields.io/badge/provenance-SLSA%20v1-2ea44f) ![Offline rebuild: verified](https://img.shields.io/badge/offline%20rebuild-verified-2ea44f)

# Fluent Bit: ReadyStack build kit

The offline build kit for the Docker image `readystack/fluentbit:v5.2.0-CE-ubuntu24.04-r2`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v5.2.0-CE-ubuntu24.04-r2](https://github.com/readystack-dev/fluentbit/releases/tag/v5.2.0-CE-ubuntu24.04-r2)
- Image: [`docker.io/readystack/fluentbit:v5.2.0-CE-ubuntu24.04-r2`](https://hub.docker.com/r/readystack/fluentbit/tags), manifest digest `sha256:e968022ca2fa8d2f6ded3e00b7338a57fe598425b2da5f382f23b19d5ecd90f7`
- Product page: https://readystack.dev/products/fluentbit
- Source and build recipe: https://readystack.dev/products/fluentbit/recipe
- Build trail: https://readystack.dev/products/fluentbit/trail/v5.2.0-CE-ubuntu24.04-r2
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=fluentbit

## The image

Single container running one C binary, /fluent-bit/bin/fluent-bit, compiled from the upstream source with every bundled library built in. It is a log, metrics and traces processor and forwarder: it reads from the inputs you configure, filters, and sends to the outputs you configure. It has no database, no web UI and no user accounts. It runs as the non-root user fluentbit.

## Who uses Fluent Bit and why

Operations and platform teams who collect logs, metrics and traces from servers, containers and devices.

Fluent Bit is a lightweight telemetry agent. It collects logs, metrics and traces from many sources, filters and processes them, and forwards them to the destinations you choose.

A typical use: A team runs applications on many servers and containers and wants all their logs in one central store. Fluent Bit on each host reads the log files, parses and filters the records, and forwards them to the team's log backend, while using little CPU and memory on the hosts.

From the upstream project's own description: https://github.com/fluent/fluent-bit, https://docs.fluentbit.io/manual/about/what-is-fluent-bit.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-20 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 22 packages with copyleft licences (GPL, LGPL, MPL), all of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`fluentbit`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-20; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/fluentbit.

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

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/fluentbit/recipe. The complete kit is the release download. Each release is a tag here. How to report a vulnerability: [SECURITY.md](SECURITY.md).

## About ReadyStack

**Stable, Open-source Releases, Automated.** ReadyStack releases popular open-source apps every week with every available critical and high security fix applied, plus full documentation, reference data, tests and a build recipe that recreates each release bit-for-bit. Each release is rebuilt from its pinned inputs with the network off, checked against its published digest, signed, and scanned and graded again every day. The images and build kits are free, and ReadyStack is soon to be a nonprofit.

[readystack.dev](https://readystack.dev) · [All products](https://readystack.dev/products.html) · [Build kits on GitHub](https://github.com/readystack-dev) · [Defect log](https://readystack.dev/defects.html) · contact@readystack.dev

— ReadyStack · https://readystack.dev
