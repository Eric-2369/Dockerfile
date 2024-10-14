FROM debian:bookworm-20240926-slim
LABEL authors="eric2369"

RUN apt-get update \
    && apt-get install -y --no-install-recommends curl ca-certificates git openssh-client xz-utils \
    && rm -rf /var/lib/apt/lists/*

ARG TARGETPLATFORM
RUN set -eux; \
    case "${TARGETPLATFORM}" in \
        "linux/amd64") \
            URL="https://nodejs.org/dist/v20.18.0/node-v20.18.0-linux-x64.tar.xz"; \
            ;; \
        "linux/arm64") \
            URL="https://nodejs.org/dist/v20.18.0/node-v20.18.0-linux-arm64.tar.xz"; \
            ;; \
        *) \
            echo "Unsupported platform: ${TARGETPLATFORM}"; \
            exit 1; \
            ;; \
    esac; \
    curl "${URL}" --location | tar --extract --xz --directory=/usr/local --strip-components=1

RUN npm install typescript@5.6.3 tsx@4.19.1 @salesforce/cli@2.61.8 -g \
    && npm cache clean --force

RUN echo y | sf plugins install sfdx-git-delta@5.47.0 \
    && npm cache clean --force

RUN apt-get update \
    && apt-get install -y --no-install-recommends chromium \
    && rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]
