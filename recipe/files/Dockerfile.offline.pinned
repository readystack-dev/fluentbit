# GENERATED L-009 hermetic C build (--network=none, SBOM+SLSA-attestation-producing).
# Pinned bases; vendored source (in-tree deps) + extra build debs + runtime debs from Dockerfile.vendor.
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289 AS cbuild
# dpkg-install the vendored EXTRA build debs (pkg-config/libssl-dev/...) — gcc/make are in the builder.
RUN --mount=type=bind,source=vendored/build-debs,target=/tmp/bdebs \
    set --; for f in /tmp/bdebs/*.deb; do [ -e "$f" ] && set -- "$@" "$f"; done; \
    for pass in 1 2 3 4 5; do [ $# -eq 0 ] || dpkg -i --skip-same-version "$@" >/dev/null 2>&1 && break; done; \
    dpkg --configure -a && bad= && for d in "$@"; do p=$(dpkg-deb -f "$d" Package) v=$(dpkg-deb -f "$d" Version); \
      case "$(dpkg-query -W -f='${Version} ${db:Status-Abbrev}' "$p" 2>/dev/null)" in "$v ii"*) ;; *) bad="$bad $p=$v" ;; esac; done && \
    { [ -z "$bad" ] || { echo "vendored debs not installed at their vendored version:$bad" >&2; false; }; }
WORKDIR /src
COPY vendored/fluentbit-src/ .
ARG SOURCE_DATE_EPOCH=1795161048
ENV SOURCE_DATE_EPOCH=${SOURCE_DATE_EPOCH}
RUN cmake -S . -B build -DFLB_RELEASE=On -DFLB_SIMD=On -DFLB_JEMALLOC=On -DFLB_TLS=On -DFLB_SHARED_LIB=Off -DFLB_EXAMPLES=Off -DFLB_HTTP_SERVER=On -DFLB_LOG_NO_CONTROL_CHARS=On -DFLB_CHUNK_TRACE=Off -DFLB_IN_SYSTEMD=On -DFLB_IN_KAFKA=On -DFLB_OUT_KAFKA=On -DFLB_OUT_PGSQL=Off -DFLB_IN_EXEC=Off -DFLB_WASM=Off -DFLB_IN_EXEC_WASI=Off -DFLB_FILTER_WASM=Off -DFLB_WAMRC=Off -DFLB_PROXY_GO=Off -DFLB_CUSTOM_CALYPTIA=Off -DFLB_IN_CALYPTIA_FLEET=Off -DFLB_OUT_CALYPTIA=Off -DFLB_ZIG=Off
RUN cmake --build build -j"$(nproc)"
RUN install -d /fluent-bit/bin /fluent-bit/etc && install -m 0755 build/bin/fluent-bit /fluent-bit/bin/fluent-bit && install -m 0644 conf/parsers.conf conf/parsers_ambassador.conf conf/parsers_java.conf conf/parsers_extra.conf conf/parsers_openstack.conf conf/parsers_cinder.conf /fluent-bit/etc/
RUN /fluent-bit/bin/fluent-bit --version > /tmp/flb.version && cat /tmp/flb.version && grep -qF 'Fluent Bit v5.2.0' /tmp/flb.version && grep -qF 'Git commit: e0149076aeff009d35fd495e55379cb78f1eb53f' /tmp/flb.version
RUN set -eu; find /fluent-bit -type f | LC_ALL=C sort | while read -r f; do \
      readelf -h "$f" >/dev/null 2>&1 || continue; \
      readelf -lW "$f" | sed -n 's/.*Requesting program interpreter: \(.*\)]$/\1/p'; \
      readelf -dW "$f" | sed -n 's/.*Shared library: \[\(.*\)\]$/\1/p'; \
    done | LC_ALL=C sort -u | while read -r lib; do case "$lib" in \
      /*) echo "$lib" ;; *) [ -n "$(find /fluent-bit -name "$lib" -print -quit)" ] || echo "$lib" ;; esac; \
    done > /kitforge.needs && echo "the install prefix loads: $(tr '\n' ' ' < /kitforge.needs)"
# rs-probe: the HEALTHCHECK helper (standard library only, from the kit's rs-probe.go)
FROM golang:1.27.3-bookworm@sha256:972b4110ba0f47539a975c31e52c64765e1ff19f2d74469ae4a595c6676e80a5 AS build_rs_probe
WORKDIR /probe
COPY rs-probe.go main.go
COPY rs-probe.mod go.mod
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOFLAGS="-buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795161048
RUN go build -trimpath -ldflags='-s -w' -o /rs-probe .
# the chiselled rootfs (vendored/chisel, cut by Dockerfile.vendor), the service user, the binaries
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289 AS rootfs
ARG SOURCE_DATE_EPOCH=1795161048
ADD vendored/chisel/rootfs.tar /rootfs/
RUN printf 'fluentbit:x:999:999::/var/lib/fluent-bit:/bin/sh\n' >> /rootfs/etc/passwd && printf 'fluentbit:x:999:\n' >> /rootfs/etc/group && \
    mkdir -p /rootfs/var/lib/fluent-bit /rootfs/var/lib/fluent-bit/storage && chown -R 999:999 /rootfs/var/lib/fluent-bit /rootfs/var/lib/fluent-bit/storage
COPY --from=build_rs_probe /rs-probe /rootfs/usr/local/bin/rs-probe
COPY fluent-bit.conf /rootfs/fluent-bit/etc/fluent-bit.conf
COPY --from=cbuild /fluent-bit /rootfs/fluent-bit
COPY --from=cbuild /kitforge.needs /tmp/needs/fluentbit
RUN chmod 755 /rootfs/usr/local/bin/rs-probe
RUN --mount=type=bind,source=vendored/chisel,target=/chisel set -eu; \
    for n in /tmp/needs/*; do while read -r lib; do [ -n "$lib" ] || continue; \
      case "$lib" in /*) [ -e "/rootfs$lib" ] ;; *) [ -n "$(find /rootfs/usr/lib /rootfs/usr/lib64 -name "$lib" 2>/dev/null | head -1)" ] ;; esac || \
      { echo "$(basename "$n") needs $lib, which the chiselled rootfs does not have: add its slice to runtime_base.slices" >&2; exit 1; }; \
    done < "$n"; done; rm -rf /tmp/needs; \
    [ -e /rootfs/etc/os-release ] || { echo "no /etc/os-release: base-files_release-info is missing" >&2; exit 1; }; \
    [ "$(grep -c '^Package: ' /rootfs/var/lib/dpkg/status)" = "$(wc -l < /chisel/packages.tsv)" ] || \
      { echo "/var/lib/dpkg/status does not list every package of vendored/chisel/packages.tsv" >&2; exit 1; }; \
    rm -rf /rootfs/var/log/dpkg.log /rootfs/var/log/alternatives.log /rootfs/var/log/apt /rootfs/var/cache/ldconfig/aux-cache /rootfs/var/cache/fontconfig
FROM scratch
ARG SOURCE_DATE_EPOCH=1795161048
COPY --from=rootfs /rootfs/ /
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin PATH=/fluent-bit/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LABEL org.opencontainers.image.licenses="Apache-2.0"
LABEL org.opencontainers.image.title="Fluent Bit"
LABEL org.opencontainers.image.source="https://github.com/fluent/fluent-bit"
LABEL org.opencontainers.image.description="Fluent Bit 5.2.0 log, metrics and traces processor (Apache-2.0), compiled from source"
LABEL org.opencontainers.image.version="v5.2.0-CE-ubuntu24.04-r2"
LABEL org.opencontainers.image.vendor="ReadyStack.dev"
LABEL readystack.product="fluentbit"
LABEL readystack.edition="CE"
LABEL readystack.build_method="source-rebuild-c"
LABEL org.opencontainers.image.revision="e0149076aeff009d35fd495e55379cb78f1eb53f"
HEALTHCHECK --interval=5s --timeout=120s --start-period=10s \
  CMD /usr/local/bin/rs-probe http://127.0.0.1:2020/api/v2/health || exit 1
WORKDIR /var/lib/fluent-bit
USER fluentbit
EXPOSE 2020
ENTRYPOINT ["/fluent-bit/bin/fluent-bit"]
CMD ["-c", "/fluent-bit/etc/fluent-bit.conf"]
