FROM debian:bookworm-20250113-slim
LABEL authors="eric2369"

RUN apt-get update \
    && apt-get install -y --no-install-recommends dumb-init curl ca-certificates unzip procps iproute2 \
    && rm -rf /var/lib/apt/lists/*

RUN useradd --create-home --shell /bin/bash serverstatus

USER serverstatus

ARG TARGETPLATFORM
RUN set -eux; \
    case "${TARGETPLATFORM}" in \
        "linux/amd64") \
            URL="https://github.com/zdz/ServerStatus-Rust/releases/download/v1.8.1/client-x86_64-unknown-linux-musl.zip"; \
            ;; \
        "linux/arm64") \
            URL="https://github.com/zdz/ServerStatus-Rust/releases/download/v1.8.1/client-aarch64-unknown-linux-musl.zip"; \
            ;; \
        *) \
            echo "Unsupported platform: ${TARGETPLATFORM}"; \
            exit 1; \
            ;; \
    esac; \
    curl "${URL}" --location --output /home/serverstatus/serverstatus.zip; \
    unzip /home/serverstatus/serverstatus.zip -d /home/serverstatus; \
    rm /home/serverstatus/serverstatus.zip

RUN echo '#!/bin/bash' > /home/serverstatus/start.sh \
    && echo 'set -euo pipefail' >> /home/serverstatus/start.sh \
    && echo 'exec /home/serverstatus/stat_client --addr "$ADDR" --user "$USER" --pass "$PASS" --interval "$INTERVAL"' >> /home/serverstatus/start.sh \
    && chmod +x /home/serverstatus/start.sh

ENV ADDR="https://ssr.rs/report"
ENV USER="h1"
ENV PASS="p1"
ENV INTERVAL="1"

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
CMD ["bash", "-c", "/home/serverstatus/start.sh"]
