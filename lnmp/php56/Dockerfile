FROM zlilizh/phpfpm5.6:latest

RUN apt-get install -y curl \
    #------- 安装 redis 扩展-------
    && curl -k -fsSL 'https://github.com/phpredis/phpredis/archive/refs/tags/4.3.0.tar.gz' -o /tmp/phpredis.tar.gz \
    && mkdir -p /tmp/phpredis && tar -xf /tmp/phpredis.tar.gz -C /tmp/phpredis --strip-components=1 \
    && cd /tmp/phpredis && phpize && ./configure && make -j$(nproc) && make install && docker-php-ext-enable redis \