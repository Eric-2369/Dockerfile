FROM debian:bullseye-20241223-slim
LABEL authors="eric2369"

RUN apt-get update \
    && apt-get install -y --no-install-recommends dumb-init curl ca-certificates git openssh-client python3 \
    && rm -rf /var/lib/apt/lists/*

RUN useradd --create-home --shell /bin/bash shadowsocksr

USER shadowsocksr

RUN git clone --branch manyuser "https://github.com/shadowsocksrr/shadowsocksr.git" /home/shadowsocksr/shadowsocksr

RUN echo '#!/bin/bash' > /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo 'cat << EOF > /home/shadowsocksr/shadowsocksr/shadowsocks/config.json' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '{' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"server":"0.0.0.0",' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"server_ipv6":"::",' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"server_port":${SERVER_PORT},' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"local_address":"127.0.0.1",' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"local_port":1080,' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"password":"${PASSWORD}",' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"timeout":60,' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"method":"${METHOD}",' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"protocol":"${PROTOCOL}",' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"protocol_param":"${PROTOCOL_PARAM}",' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"obfs":"${OBFS}",' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"obfs_param":"${OBFS_PARAM}",' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"redirect":"",' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"dns_ipv6":${DNS_IPV6},' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"fast_open":false,' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '"workers":1' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo '}' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo 'EOF' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && echo 'python3 /home/shadowsocksr/shadowsocksr/shadowsocks/server.py -c /home/shadowsocksr/shadowsocksr/shadowsocks/config.json' >> /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh \
    && chmod +x /home/shadowsocksr/shadowsocksr/shadowsocks/start.sh

ENV SERVER_PORT="8388"
ENV PASSWORD="m"
ENV METHOD="aes-128-ctr"
ENV PROTOCOL="auth_aes128_md5"
ENV PROTOCOL_PARAM=""
ENV OBFS="tls1.2_ticket_auth_compatible"
ENV OBFS_PARAM=""
ENV DNS_IPV6="false"
ENV TZ="Asia/Shanghai"

ENTRYPOINT ["/usr/bin/dumb-init", "--"]
CMD ["bash", "-c", "/home/shadowsocksr/shadowsocksr/shadowsocks/start.sh"]
