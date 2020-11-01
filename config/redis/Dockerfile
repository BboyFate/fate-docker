ARG REDIS_VERSION
FROM redis:${REDIS_VERSION}
MAINTAINER Fate bboyfate@163.com

# set timezome
ARG TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# CMD ["redis-server", "/usr/local/etc/redis/redis.conf"]
CMD ["redis-server"]
