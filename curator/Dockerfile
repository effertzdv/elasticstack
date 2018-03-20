FROM python:2.7.14-jessie

# grab gosu for easy step-down from root
# proxy ?

#ENV http_proxy=http://my.proxy:3128
#ENV https_proxy=http://my.proxy:3128

RUN gpg --keyserver hkp://p80.pool.sks-keyservers.net:80 --keyserver-options "timeout=10 http-proxy=$http_proxy" --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4
RUN arch="$(dpkg --print-architecture)" \
	&& set -x \
	&& curl -o /usr/local/bin/gosu -fSL "https://github.com/tianon/gosu/releases/download/1.3/gosu-$arch" \
	&& curl -o /usr/local/bin/gosu.asc -fSL "https://github.com/tianon/gosu/releases/download/1.3/gosu-$arch.asc" \
	&& gpg --verify /usr/local/bin/gosu.asc \
	&& rm /usr/local/bin/gosu.asc \
	&& chmod +x /usr/local/bin/gosu

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN groupadd -r curator && useradd -r -g curator curator

RUN pip install elasticsearch-curator==5.4.0

COPY docker-entrypoint.sh /

ENV INTERVAL_IN_HOURS=24

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["curator"] 

