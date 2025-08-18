FROM debian:bookworm-slim

RUN set -eux; \
    apt-get update && apt-get install -y --no-install-recommends \
      inotify-tools curl bash ca-certificates procps \
    && rm -rf /var/lib/apt/lists/*

COPY lcpencrypt /usr/local/bin/lcpencrypt
COPY asset-encrypter.sh /usr/local/bin/asset-encrypter.sh
RUN chmod +x /usr/local/bin/lcpencrypt /usr/local/bin/asset-encrypter.sh

RUN mkdir -p /assets /assets-encrypted && \
    chmod 755 /assets /assets-encrypted

ENTRYPOINT ["/usr/local/bin/asset-encrypter.sh"]