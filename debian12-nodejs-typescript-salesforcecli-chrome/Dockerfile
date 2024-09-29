FROM debian:bookworm-20240812-slim
LABEL authors="eric2369"

RUN apt-get update \
    && apt-get install -y --no-install-recommends curl ca-certificates git openssh-client xz-utils \
    && rm -rf /var/lib/apt/lists/*

ARG TARGETPLATFORM
RUN set -eux; \
    case "${TARGETPLATFORM}" in \
        "linux/amd64") \
            ARCH='x64'; \
            ;; \
        "linux/arm64") \
            ARCH='arm64'; \
            ;; \
        *) \
            echo "Unsupported platform: ${TARGETPLATFORM}"; \
            exit 1; \
            ;; \
    esac; \
    curl https://nodejs.org/dist/v20.17.0/node-v20.17.0-linux-${ARCH}.tar.xz | tar -xJC /usr/local --strip-components=1

RUN npm install typescript tsx @salesforce/cli -g \
    && npm cache clean --force

RUN echo y | sf plugins install sfdx-git-delta \
    && npm cache clean --force

RUN curl https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb -o /tmp/google-chrome-stable_current_amd64.deb \
    && apt-get update \
    && apt-get install -y --no-install-recommends /tmp/google-chrome-stable_current_amd64.deb \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/google-chrome-stable_current_amd64.deb

CMD ["/bin/bash"]
