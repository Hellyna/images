ARG ALPINE_VERSION=latest

FROM alpine:${ALPINE_VERSION} as builder

ARG SYNAPSE_VERSION=v1.36.0

RUN apk add --no-cache \
  build-base \
  git

RUN set -eux; \
  git clone https://github.com/matrix-org/synapse synapse-repo; \
  cd synapse-repo; \
  git checkout "$SYNAPSE_VERSION"

ARG MAKEFLAGS=-j13

RUN set -eux; \
  wget -O - https://github.com/jemalloc/jemalloc/releases/download/5.2.1/jemalloc-5.2.1.tar.bz2 | tar -xj; \
    cd jemalloc-5.2.1; \
    ./configure; \
    make; \
    make install

FROM hellyna/alpine-base:${ALPINE_VERSION}

LABEL org.opencontainers.image.url='https://matrix.org/docs/projects/server/synapse'
LABEL org.opencontainers.image.documentation='https://github.com/matrix-org/synapse/blob/master/docker/README.md'
LABEL org.opencontainers.image.source='https://github.com/matrix-org/synapse.git'
LABEL org.opencontainers.image.licenses='Apache-2.0'

RUN apk add --no-cache \
  libgcc \
  libstdc++ \
  synapse

COPY --from=builder /synapse-repo/docker/start.py /start.py
COPY --from=builder /synapse-repo/docker/conf /conf
# Mimic debian file directories since we are using upstream scripts that uses debian
#COPY --from=builder /usr/local/lib/libjemalloc.so.2 /usr/local/lib/
COPY --from=builder /usr/local/lib/libjemalloc.so.2 /usr/lib/x86_64-linux-gnu/libjemalloc.so.2
RUN set -eux; \
  ln -sT /usr/bin/python3 /usr/local/bin/python; \
  ln -sT /usr/local/bin/gosu /usr/sbin/gosu; \
  busybox --help >/dev/null;

ENV PYTHONPATH=/usr/local/lib/python3.8/site-packages LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.2

ENTRYPOINT ["/start.py"]
HEALTHCHECK --start-period=5s --interval=15s --timeout=5s \
    CMD curl -fSs http://localhost:8008/health || exit 1
