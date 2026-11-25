# Fluent Bit v5.2.0 — ReadyStack release `v5.2.0-CE-ubuntu24.04-r2`

Generated 2026-11-20 from this release's build records and strategy. Statements that come from a single record name it in brackets; the others come from this release's strategy and build log.

## Release

- **Image:** `readystack/fluentbit:v5.2.0-CE-ubuntu24.04-r2` (linux/amd64). Index digest `sha256:a0a247151bb3fb16f8cc96a5a6b4499e11e11a510cccd70ffacf6e169ced4129` [`docs/provenance/LOCKED_DIGEST.txt`].
- **Image manifest digest:** `sha256:e968022ca2fa8d2f6ded3e00b7338a57fe598425b2da5f382f23b19d5ecd90f7` [`TARGET_DIGEST.txt`].
- **Upstream:** Fluent Bit v5.2.0 built from source at `v5.2.0` of https://github.com/fluent/fluent-bit — upstream notes: https://github.com/fluent/fluent-bit/releases/tag/v5.2.0 [`docs/provenance/BUILD_RECIPE.json`].
- **Base image:** none (`FROM scratch`): a chiselled Ubuntu 24.04 runtime, the files Canonical's chisel cut from Ubuntu's own packages (`vendored/chisel/slices.txt`, `vendored/chisel/packages.tsv`); runtime user `fluentbit`.
- **Build:** `source-rebuild-c`, compiled offline with `--network=none`.
- **Supersedes:** ReadyStack release `v5.2.0-CE-ubuntu24.04-r1`.

## What changed in this release

- Upstream version unchanged (v5.2.0); ReadyStack rebuild.
- Fluent Bit v5.2.0 is unchanged from the release this supersedes (v5.2.0-CE-ubuntu24.04-r1); this release rebuilds it. Changes: the image is rebuilt on the current ubuntu:24.04 image (sha256:9c7f200c07e3…), which its maintainers updated after v5.2.0-CE-ubuntu24.04-r1 was built on sha256:2198e4c65076…. Settings, ports and data volumes are those of v5.2.0-CE-ubuntu24.04-r1: replace the container with this image on the same volumes.

## Security status at release (measured 2026-11-20)

- **Docker Scout:** 0 fixable critical/high vulnerabilities; runs as `fluentbit` (non-root); SBOM + provenance attestations present (SPDX SBOM + SLSA v1) [`docs/provenance/SCOUT_VERDICT.json`].
- **OSV cross-check:** no fixable critical/high vulnerability beyond Docker Scout's findings.
- **Signature:** cosign signature verified by ReadyStack's release gate before publishing; check it yourself with `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/fluentbit:v5.2.0-CE-ubuntu24.04-r2` (signatures are recorded in the public Sigstore transparency log).
- **ReadyStack release check at release:** passed (ReadyStack's own test, not a Docker Scout grade). Vulnerabilities are disclosed every day and the image never changes, so it is re-checked daily, beside Docker Scout's own dated grade, at https://readystack.dev/queue.
- **Before exposing this service:** The monitoring API on port 2020 has no authentication and no TLS: it exposes build information, plugin names, metrics and health. Publish it on 127.0.0.1 or keep it on a private network.
- **Before exposing this service:** If you turn on hot_reload, POST /api/v2/reload on the same unauthenticated port makes Fluent Bit reload its configuration; keep the port private (src/http_server/api/v2/reload.c).
- **Before exposing this service:** The container runs as the non-root user fluentbit and cannot read host files that only root can read (for example /var/lib/docker/containers/*/*-json.log). Grant read access to its group, or run it with --user 0 knowingly.

## Reproducibility

Rebuilds offline from this archive to the identical image digest `sha256:e968022ca2fa8d2f6ded3e00b7338a57fe598425b2da5f382f23b19d5ecd90f7`: run `./BUILD_OFFLINE.sh`, then `./VERIFY_OFFLINE.sh` (add `--online` to confirm the Docker Hub tag holds the same digest). Before publishing, ReadyStack rebuilt the kit with `./BUILD_OFFLINE.sh` and compared the result with the published digest [`docs/provenance/REPRO_VERIFICATION.json`], then ran `./BUILD_OFFLINE.sh` and `./VERIFY_OFFLINE.sh` from this archive (without `--online`).

## Validation

- 4 of 4 use cases passed (4 authored) against the running image [`docs/validation/test-results.json`].

## Known issues

- **medium** (upstream): Unauthenticated monitoring API on port 2020 by default [`RSD-fluentbit-5d92c6393a`]

## Defects fixed

- 3 problems found before release and fixed before it shipped (3 by review); each is listed in `docs/defects/defects.json`.

## Verify this archive

```
sha256sum -c CHECKSUMS.sha256
./BUILD_OFFLINE.sh && ./VERIFY_OFFLINE.sh --online
cosign verify --key https://readystack.dev/keys/cosign.pub readystack/fluentbit:v5.2.0-CE-ubuntu24.04-r2
```

ReadyStack records every signature in the public Sigstore transparency log (Rekor), so the command needs no extra flags.

