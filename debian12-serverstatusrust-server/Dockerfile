FROM debian:bookworm-20240926-slim
LABEL authors="eric2369"

RUN apt-get update \
    && apt-get install -y --no-install-recommends curl ca-certificates unzip \
    && rm -rf /var/lib/apt/lists/*

RUN useradd -m -s /bin/bash serverstatus

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

EXPOSE 8080

CMD /home/serverstatus/stat_server -c /home/serverstatus/config.toml
