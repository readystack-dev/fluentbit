# Provenance — Fluent Bit (`v5.2.0-CE-ubuntu24.04-r2`)

Every item below is copied from a record in this archive; the record is named in brackets.

## Source

- Upstream repository: https://github.com/fluent/fluent-bit
- Revision built: `v5.2.0` (commit `e0149076aeff009d35fd495e55379cb78f1eb53f`)
- Source tree: `79d68878a23c00b215452f202735042d5e0c4b7257e1d81d33632bc969c9a82a` over 12898 files [`docs/provenance/SOURCE_TREE_HASH.json`]
- Community Edition only: enterprise code is removed or refused before the build (CE audit at acquire).

## Build

- Method: `source-rebuild-c`; base image `scratch + a chiselled Ubuntu 24.04 runtime (vendored/chisel/slices.txt, packages.tsv)`
- Compiled with `--network=none` from vendored source, dependencies and base images; BuildKit pinned to `moby/buildkit:v0.31.2` (shipped in `toolchain/`).
- SOURCE_DATE_EPOCH: `1795161048` [`SOURCE_DATE_EPOCH.txt`]
- Frozen inputs: [`docs/provenance/FREEZE_MANIFEST.json`], build recipe: [`docs/provenance/BUILD_RECIPE.json`].

## Output

- Published image: `readystack/fluentbit:v5.2.0-CE-ubuntu24.04-r2` — index digest `sha256:a0a247151bb3fb16f8cc96a5a6b4499e11e11a510cccd70ffacf6e169ced4129` [`docs/provenance/LOCKED_DIGEST.txt`]
- linux/amd64 image digest `sha256:e968022ca2fa8d2f6ded3e00b7338a57fe598425b2da5f382f23b19d5ecd90f7` — reproduced from this archive before publishing [`docs/provenance/REPRO_VERIFICATION.json`].

## Attestations and signature

- SBOM + SLSA provenance attached to the image: attached: an SPDX SBOM and SLSA v1 provenance [`docs/provenance/SCOUT_VERDICT.json`]
- cosign signature by the ReadyStack release key: verified before publishing. Check it: `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/fluentbit:v5.2.0-CE-ubuntu24.04-r2`

## Pinned inputs, and why

- 23 pinned inputs, each with where it came from, why this one, how it was checked and when it was decided: `docs/PINS.md` [`docs/provenance/PINS.json`] (upstream source 2, base and build images 3, image digests 2, dependency versions readystack sets 3, pins in the kit's dockerfiles 5, readystack's build engine 6, dependencies upstream chose 2).

## Build trail

- How this release passed through the ReadyStack harness, record by record (every chain, drive, stage, review round, defect and check, each naming the record it is copied from): `ReadyStack-fluentbit-v5.2.0-CE-ubuntu24.04-r2.trail.json`, published beside this archive and signed with the release key (`ReadyStack-fluentbit-v5.2.0-CE-ubuntu24.04-r2.trail.json.sigstore`), and readable at https://readystack.dev/products/fluentbit/trail/v5.2.0-CE-ubuntu24.04-r2.

