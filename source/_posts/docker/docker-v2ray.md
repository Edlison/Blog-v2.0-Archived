---
title: 制作V2ray的Dockerfile
categories:
- Notebook
tags:
- docker
- v2ray
---

# Dockerfile

```dockerfile
FROM ubuntu:latest

WORKDIR /root
COPY v2ray.sh /root

RUN mkdir -p /etc/v2ray /usr/local/share/v2ray /var/log/v2ray \
					&& chmod +x /root/v2ray.sh \
					&& /root/v2ray.sh

VOLUME /etc/v2ray
CMD [ "v2ray", "-config", "/etc/v2ray/config.json" ]
```

# sh

```sh
apt update \
&& apt install wget zip \
&& wget https://github.com/v2fly/v2ray-core/releases/download/${TAG}/${V2RAY_FILE} \
&& unzip v2ray.zip \
&& chmod +x v2ray v2ctl \
&& mv v2ray v2ctl /usr/bin \
&& mv geosite.dat geoip.dat /usr/local/share/v2ray \
&& mv config.json /etc/v2ray/config.json

echo 'Done'
```

