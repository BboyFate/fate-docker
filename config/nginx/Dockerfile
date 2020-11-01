ARG NGINX_VERSION
FROM nginx:${NGINX_VERSION}

MAINTAINER Fate bboyfate@163.com

ARG TZ
# set timezome
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

WORKDIR /var/www

CMD ["nginx", "-g", "daemon off;"]
