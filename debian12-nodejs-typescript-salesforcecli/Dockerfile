FROM debian:bookworm-20250428-slim
LABEL authors="eric2369"

RUN apt-get update \
    && apt-get install -y --no-install-recommends dumb-init curl ca-certificates git openssh-client xz-utils \
    && rm -rf /var/lib/apt/lists/*

ARG TARGETPLATFORM
RUN set -eux; \
    case "${TARGETPLATFORM}" in \
        "linux/amd64") \
            URL="https://nodejs.org/dist/v22.15.1/node-v22.15.1-linux-x64.tar.xz"; \
            ;; \
        "linux/arm64") \
            URL="https://nodejs.org/dist/v22.15.1/node-v22.15.1-linux-arm64.tar.xz"; \
            ;; \
        *) \
            echo "Unsupported platform: ${TARGETPLATFORM}"; \
            exit 1; \
            ;; \
    esac; \
    curl "${URL}" --location | tar --extract --xz --directory=/usr/local --strip-components=1

RUN npm install typescript@5.8.3 tsx@4.19.4 @salesforce/cli@2.88.6 -g \
    && npm cache clean --force

RUN mkdir -p /root/.config/sf \
    && echo '["sfdx-git-delta"]' > /root/.config/sf/unsignedPluginAllowList.json \
    && sf plugins install sfdx-git-delta@6.8.0 \
    && npm cache clean --force

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
CMD ["/bin/bash"]
