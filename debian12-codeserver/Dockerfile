FROM debian:bookworm-20250317-slim
LABEL authors="eric2369"

RUN apt-get update \
    && apt-get install -y --no-install-recommends dumb-init curl ca-certificates git openssh-client sudo wget vim zip unzip htop procps \
    && rm -rf /var/lib/apt/lists/*

RUN useradd --create-home --shell /bin/bash eric2369 \
    && echo "eric2369 ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/nopasswd

ARG TARGETPLATFORM
RUN set -eux; \
    case "${TARGETPLATFORM}" in \
        "linux/amd64") \
            URL="https://github.com/coder/code-server/releases/download/v4.98.2/code-server_4.98.2_amd64.deb"; \
            ;; \
        "linux/arm64") \
            URL="https://github.com/coder/code-server/releases/download/v4.98.2/code-server_4.98.2_arm64.deb"; \
            ;; \
        *) \
            echo "Unsupported platform: ${TARGETPLATFORM}"; \
            exit 1; \
            ;; \
    esac; \
    curl "${URL}" --location --output /home/eric2369/code-server.deb; \
    dpkg -i /home/eric2369/code-server.deb; \
    rm /home/eric2369/code-server.deb

USER eric2369

RUN set -eux; \
    case "${TARGETPLATFORM}" in \
        "linux/amd64") \
            URL="https://repo.anaconda.com/miniconda/Miniconda3-py312_25.1.1-2-Linux-x86_64.sh"; \
            ;; \
        "linux/arm64") \
            URL="https://repo.anaconda.com/miniconda/Miniconda3-py312_25.1.1-2-Linux-aarch64.sh"; \
            ;; \
    esac; \
    curl "${URL}" --location --output /home/eric2369/miniconda.sh; \
    bash /home/eric2369/miniconda.sh -b -p /home/eric2369/miniconda; \
    rm /home/eric2369/miniconda.sh; \
    echo 'export PATH="/home/eric2369/miniconda/bin:$PATH"' >> /home/eric2369/.bashrc

RUN curl "https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh" --location --output - | bash

RUN curl "https://get.sdkman.io" --location --output - | bash

RUN curl "https://sh.rustup.rs" --location --output - | bash -s -- -y --default-toolchain none

RUN echo "#!/bin/bash" > /home/eric2369/start.sh \
    && echo "set -euo pipefail" >> /home/eric2369/start.sh \
    && echo "exec /usr/bin/code-server --bind-addr 0.0.0.0:8080 --auth password --disable-telemetry --disable-update-check /home/eric2369/project" >> /home/eric2369/start.sh \
    && chmod +x /home/eric2369/start.sh

ENV PASSWORD="Eric2369"
VOLUME /home/eric2369/project
EXPOSE 8080

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
CMD ["bash", "-c", "/home/eric2369/start.sh"]
