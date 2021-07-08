ARG ALPINE_VERSION=3.14

FROM hellyna/alpine-base:${ALPINE_VERSION}

LABEL org.opencontainers.image.url='https://github.com/hellyna/images/postgres'
LABEL org.opencontainers.image.documentation='https://github.com/hellyna/images/postgres/README.md'
LABEL org.opencontainers.image.source='https://github.com/hellyna/images/postgres'
LABEL org.opencontainers.image.licenses='Apache-2.0'

ENV PGDATA /var/lib/postgresql/data

RUN apk add --no-cache bash postgresql tzdata && \
  mkdir -p /docker-entrypoint-initdb.d && \
  install -o postgres -g postgres -dm2777 /var/run/postgresql && \
  install -o postgres -g postgres -dm777 "$PGDATA"

COPY docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh

STOPSIGNAL SIGINT
EXPOSE 5432

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["postgres"]
