FROM nginx:1.27-alpine

RUN apk update && apk upgrade && \
    apk --update add logrotate openssl bash && \
    apk add --no-cache certbot certbot-nginx

RUN apk del --no-cache

COPY ./default.conf /etc/nginx/conf.d/default.conf
