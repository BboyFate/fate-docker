ARG NODE_VERSION
FROM node:${NODE_VERSION}

MAINTAINER Fate bboyfate@163.com

# # set timezome
# ARG TZ
# RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
#   && echo $TZ > /etc/timezone

# 备份原始文件 修改为国内镜像源
RUN cp /etc/apk/repositories /etc/apk/repositories.bak \
  && echo "http://mirrors.aliyun.com/alpine/latest-stable/main/" > /etc/apk/repositories \
  && apk upgrade \
  && apk add git libpng-dev autoconf automake libtool

RUN npm config set registry https://registry.npm.taobao.org --global \
    && npm install -g plop

# Clean up
RUN rm -rf /var/lib/apk/lists/* /tmp/* /var/tmp/*

WORKDIR /var/www
