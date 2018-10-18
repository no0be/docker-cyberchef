FROM alpine:3.8

# --- NGINX ---
RUN apk add --no-cache nginx \
    && mkdir -p /var/www \
    && mkdir -p /run/nginx \
    && touch /run/nginx/nginx.pid \
    && rm -f /var/log/nginx/access.log \
    && rm -f /var/log/nginx/error.log \
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log \
    && echo "server {" > /etc/nginx/conf.d/default.conf \
    && echo "	listen 80 default_server;" >> /etc/nginx/conf.d/default.conf \
    && echo "	listen [::]:80 default_server;" >> /etc/nginx/conf.d/default.conf \
    && echo "" >> /etc/nginx/conf.d/default.conf \
    && echo "	root /var/www;" >> /etc/nginx/conf.d/default.conf \
    && echo "	index index.html;" >> /etc/nginx/conf.d/default.conf \
    && echo "" >> /etc/nginx/conf.d/default.conf \
    && echo "	autoindex off;" >> /etc/nginx/conf.d/default.conf \
    && echo "	location / {}" >> /etc/nginx/conf.d/default.conf \
    && echo "}" >> /etc/nginx/conf.d/default.conf

# --- NODEJS ---
RUN apk add --no-cache nodejs nodejs-npm \
    && npm install -g grunt-cli \
    && npm cache clean --force

# --- CYBERCHEF ---
RUN apk add --no-cache git \
    && git clone https://github.com/gchq/CyberChef.git \
    && cd /CyberChef \
    && git checkout tags/v7.11.1 \
    && npm install \
    && grunt prod \
    && cp -r /CyberChef/build/prod/* /var/www \
    && cd / \
    && rm -rf /tmp/* \
    && rm -rf /CyberChef \
    && npm cache clean --force


EXPOSE 80
STOPSIGNAL SIGTERM
CMD ["nginx", "-g", "daemon off;"]
