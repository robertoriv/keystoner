FROM nginx

MAINTAINER Roberto Rivera <roberto.rivera@gmail.com>

# copy static assets, so that we can server them with nginx
COPY ./assets/ /usr/share/nginx/html/assets/

# the worker process runs with user nginx; we need to open up permissions to the assets folder
RUN chmod -R 755 /usr/share/nginx/html/assets/

# general config files, setup for proxy passing to the app
COPY ./etc/nginx/conf.d/* /etc/nginx/conf.d/
COPY ./etc/nginx/nginx.conf /etc/nginx/nginx.conf

RUN nginx