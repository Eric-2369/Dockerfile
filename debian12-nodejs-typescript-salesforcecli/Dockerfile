FROM debian:bookworm-slim
LABEL authors="eric2369"

ENV TZ="Asia/Shanghai"

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update \
    && apt-get install -y curl git xz-utils \
    && rm -rf /var/lib/apt/lists/*

RUN curl https://nodejs.org/dist/v18.20.2/node-v18.20.2-linux-x64.tar.xz | tar -xJC /usr/local --strip-components=1

RUN npm install typescript tsx @salesforce/cli -g

RUN echo y | sf plugins install sfdx-git-delta

CMD ["/bin/bash"]
