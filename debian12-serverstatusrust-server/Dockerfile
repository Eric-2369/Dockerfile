FROM debian:bookworm-20240926-slim
LABEL authors="eric2369"

RUN apt-get update \
    && apt-get install -y --no-install-recommends curl ca-certificates unzip \
    && rm -rf /var/lib/apt/lists/*

RUN useradd --create-home --shell /bin/bash serverstatus

USER serverstatus

ARG TARGETPLATFORM
RUN set -eux; \
    case "${TARGETPLATFORM}" in \
        "linux/amd64") \
            ARCH='x86_64-unknown-linux-musl'; \
            ;; \
        "linux/arm64") \
            ARCH='aarch64-unknown-linux-musl'; \
            ;; \
        *) \
            echo "Unsupported platform: ${TARGETPLATFORM}"; \
            exit 1; \
            ;; \
    esac; \
    curl "https://github.com/zdz/ServerStatus-Rust/releases/download/v1.8.1/server-${ARCH}.zip" --location --output /home/serverstatus/serverstatus.zip; \
    unzip /home/serverstatus/serverstatus.zip -d /home/serverstatus; \
    rm /home/serverstatus/serverstatus.zip

RUN mkdir -p /home/serverstatus/config \
    && mv /home/serverstatus/config.toml /home/serverstatus/config/config.toml \
    && echo '#!/bin/bash' > /home/serverstatus/start.sh \
    && echo 'exec /home/serverstatus/stat_server -c /home/serverstatus/config/config.toml' >> /home/serverstatus/start.sh \
    && chmod +x /home/serverstatus/start.sh

VOLUME /home/serverstatus/config
EXPOSE 8080

CMD ["/home/serverstatus/start.sh"]
