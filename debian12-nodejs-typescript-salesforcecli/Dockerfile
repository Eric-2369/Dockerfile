FROM debian:bookworm-20240513-slim
LABEL authors="eric2369"

ENV TZ="Asia/Shanghai"
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update \
    && apt-get install -y --no-install-recommends curl ca-certificates git openssh-client xz-utils cowsay \
    && rm -rf /var/lib/apt/lists/*

RUN curl https://nodejs.org/dist/v20.14.0/node-v20.14.0-linux-x64.tar.xz | tar -xJC /usr/local --strip-components=1

RUN npm install typescript tsx @salesforce/cli -g \
    && npm cache clean --force

RUN echo y | sf plugins install sfdx-git-delta \
    && npm cache clean --force

CMD ["/bin/bash"]
