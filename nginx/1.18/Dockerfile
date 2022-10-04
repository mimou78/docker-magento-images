FROM nginx:1.18-alpine

ARG GROUP_ID=10000
ARG APP_ID=10000

RUN apk add shadow
RUN /usr/sbin/groupadd -g ${GROUP_ID} app
RUN /usr/sbin/useradd -s /bin/sh -g ${GROUP_ID} -u ${APP_ID} app


RUN mkdir -p /var/run

RUN touch /var/run/nginx.pid
RUN mkdir /sock

RUN apk add --no-cache \
  curl \
  nss-tools \
  openssl

RUN mkdir /etc/nginx/certs \
  && echo -e "\n\n\n\n\n\n\n" | openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/certs/nginx.key -out /etc/nginx/certs/nginx.crt

ARG TARGETARCH

RUN cd /usr/local/bin/ \
  && curl -L https://github.com/FiloSottile/mkcert/releases/download/v1.4.3/mkcert-v1.4.3-linux-$TARGETARCH -o mkcert \
  && chmod +x mkcert

COPY ./conf/nginx.conf /etc/nginx/
COPY ./conf/default.conf /etc/nginx/conf.d/

RUN mkdir -p /etc/nginx/html /var/www/html /var/cache \
  && chown -R app:app /etc/nginx /var/www /var/cache/nginx /var/run/nginx.pid /sock

EXPOSE 8443

USER app:app

VOLUME /var/www

WORKDIR /var/www/html
