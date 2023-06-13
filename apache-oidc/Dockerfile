# Build step
FROM alpine:3.17.3 AS build
RUN apk add --no-cache openssl
COPY ./conf conf
RUN openssl req -x509 -newkey rsa:4096 -keyout server.key -out server.crt -days 365 -nodes -config conf

# Package step
FROM httpd:2.4-bullseye

## Enable SSL and url rewrite in the configuration
RUN sed -i \
		-e 's/^#\(Include .*httpd-ssl.conf\)/\1/' \
		-e 's/^#\(LoadModule .*mod_ssl.so\)/\1/' \
		-e 's/^#\(LoadModule .*mod_socache_shmcb.so\)/\1/' \
		-e 's/^#\(LoadModule .*mod_rewrite.so\)/\1/' \
		conf/httpd.conf

# Copy SSL certificates in the container
COPY --from=build server.crt /usr/local/apache2/conf/server.crt
COPY --from=build server.key /usr/local/apache2/conf/server.key

## Append sso.conf at the end of httpd.conf
RUN echo "\n\n#Custom configuration\n\n" >> /usr/local/apache2/conf/httpd.conf
COPY ./sso.conf /tmp/tmp.conf

RUN cat /tmp/tmp.conf >> /usr/local/apache2/conf/httpd.conf && rm -rf /tmp.conf
