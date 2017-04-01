# DESCRIPTION: configure and run nginx 1.11.10
# AUTHOR: aglorei
# COMMENTS:
# This uses ngx_http_stub_status_module to send collectd usage information, and
# provides an endpoint for a grafana dashboard. It requires an SSL certificate
# from a provider.
# USAGE:
# Build nginx image
# docker build -t turtle-nginx
#
# Run nginx
# docker run --name turtle-nginx \
#   -e COLLECTD_HOST=<collectd_host> \
#   -e GRAFANA_HOST=<grafana_host> \
#   -e GRAFANA_PORT=<grafana_port> \
#   -v </path/to/ssl/certs/server.crt>:/etc/ssl/certs/server.crt:ro \
#   -v </path/to/ssl/private/server.pem>:/etc/ssl/private/server.key:ro \
#   -v </path/to/ssl/private/ca-certs.pem>:/etc/ssl/private/ca-certs.pem:ro \
#   -v </path/to/dhparam.pem>:/etc/ssl/certs/dhparam.pem:ro \
#   -p 80:80 -p 443:443 -d --restart=always turtle-nginx:latest

# Base Image
FROM nginx:1.11.10

# Install j2cli
RUN apt-get update && \
  apt-get install -y python-setuptools && \
  easy_install j2cli && \
  apt-get purge -y --auto-remove && \
  rm -rf /var/lib/apt/lists/*

# Copy over templates
COPY aglorei_nginx.conf.j2 /templates/
COPY docker_entrypoint.sh /

# Entrypoint and CMD
ENTRYPOINT ["/docker_entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]