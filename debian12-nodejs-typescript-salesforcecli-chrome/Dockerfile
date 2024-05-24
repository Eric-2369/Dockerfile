FROM debian:bookworm-slim
LABEL authors="eric2369"

ENV TZ="Asia/Shanghai"

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update \
    && apt-get install -y curl git xz-utils ffmpeg \
    && rm -rf /var/lib/apt/lists/*

RUN curl https://nodejs.org/dist/v20.13.1/node-v20.13.1-linux-x64.tar.xz | tar -xJC /usr/local --strip-components=1

RUN npm install typescript tsx @salesforce/cli -g

RUN echo y | sf plugins install sfdx-git-delta

RUN curl https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb -o /tmp/google-chrome-stable_current_amd64.deb \
    && apt-get update \
    && apt-get install -y /tmp/google-chrome-stable_current_amd64.deb \
    && rm -rf /var/lib/apt/lists/* /var/cache/apt/* \
    && rm -rf /tmp/google-chrome-stable_current_amd64.deb

CMD ["/bin/bash"]
