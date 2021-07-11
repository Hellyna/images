ARG ALPINE_VERSION=latest

FROM alpine:${ALPINE_VERSION}

LABEL org.opencontainers.image.url='https://github.com/hellyna/images'
LABEL org.opencontainers.image.documentation='https://github.com/hellyna/images'
LABEL org.opencontainers.image.source='https://github.com/hellyna/images'
LABEL org.opencontainers.image.licenses='Apache-2.0'

RUN apk add --no-cache inotify-tools jq

COPY /entrypoint /usr/local/bin/entrypoint
COPY /acme2pem /usr/local/bin/acme2pem

ENTRYPOINT ["entrypoint"]

