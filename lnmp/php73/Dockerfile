FROM php:7.3.14-fpm
RUN apt-get update -y && apt-get install -y \
    apt-utils libssl-dev cmake make autoconf automake curl gcc openssl \
    #源码中存在的库，允许直接使用 docker 安装
    #------- 安装GD库，自定义安装配置 php版本可能会影响扩展库参数 -------
	libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libpng-dev libjpeg-dev \
	&& docker-php-ext-configure gd --with-jpeg-dir=/usr/include/ --with-freetype-dir=/usr/include/ && docker-php-ext-install gd && docker-php-ext-enable gd \
	#------- 源码中存在的扩展可直接安装 -------
	&& docker-php-ext-install mysqli \
	&& docker-php-ext-install pdo_mysql \
    #------- 安装 composer -------
    && curl -k -fsSL 'https://install.phpcomposer.com/installer' -o /tmp/composer-setup.php && cd /tmp && php composer-setup.php && mv composer.phar /usr/local/bin/composer \
    #源码不存在的库可先下载后使用phpize编译安装，并使用docker-php-ext-enable命令添加扩展进入php.ini配置目录中
    #------- 安装 redis 扩展-------
    && curl -k -fsSL 'https://github.com/phpredis/phpredis/archive/refs/tags/4.3.0.tar.gz' -o /tmp/phpredis.tar.gz \
    && mkdir -p /tmp/phpredis && tar -xf /tmp/phpredis.tar.gz -C /tmp/phpredis --strip-components=1 \
    && cd /tmp/phpredis && phpize && ./configure && make -j$(nproc) && make install && docker-php-ext-enable redis \
    #------- 安装 swoole 扩展-------
    && curl -k -fsSL 'https://github.com/swoole/swoole-src/archive/v4.4.23.tar.gz' -o /tmp/swoole.tar.gz \
    && mkdir -p /tmp/swoole && tar -xf /tmp/swoole.tar.gz -C /tmp/swoole --strip-components=1 \
    && cd /tmp/swoole && phpize && ./configure --enable-openssl --enable-http2 && make -j$(nproc) && make install && docker-php-ext-enable swoole \
    && echo "构建完成！"