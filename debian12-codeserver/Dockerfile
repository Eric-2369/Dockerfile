FROM debian:bookworm-20240926-slim
LABEL authors="eric2369"

RUN apt-get update \
    && apt-get install -y --no-install-recommends curl ca-certificates git openssh-client dumb-init sudo wget vim htop procps \
    && rm -rf /var/lib/apt/lists/*

RUN useradd --create-home --shell /bin/bash eric2369 \
    && echo "eric2369 ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/nopasswd

ARG TARGETPLATFORM
RUN set -eux; \
    case "${TARGETPLATFORM}" in \
        "linux/amd64") \
            ARCH='amd64'; \
            ;; \
        "linux/arm64") \
            ARCH='arm64'; \
            ;; \
        *) \
            echo "Unsupported platform: ${TARGETPLATFORM}"; \
            exit 1; \
            ;; \
    esac; \
    curl https://github.com/coder/code-server/releases/download/v4.93.1/code-server_4.93.1_${ARCH}.deb --location --output /home/eric2369/code-server.deb; \
    dpkg -i /home/eric2369/code-server.deb; \
    rm /home/eric2369/code-server.deb

RUN echo '#!/bin/sh' > /home/eric2369/start.sh \
    && echo 'set -eu' >> /home/eric2369/start.sh \
    && echo 'exec dumb-init /usr/bin/code-server --bind-addr 0.0.0.0:8080 --auth password --disable-telemetry --disable-update-check /home/eric2369/project' >> /home/eric2369/start.sh \
    && chmod +x /home/eric2369/start.sh

VOLUME /home/eric2369/project
EXPOSE 8080

USER eric2369
ENV PASSWORD="Eric2369"

CMD ["/home/eric2369/start.sh"]
