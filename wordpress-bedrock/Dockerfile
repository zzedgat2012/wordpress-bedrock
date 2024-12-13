# Use PHP-FPM as base image
FROM php:8.1-fpm

# Logo
LABEL org.opencontainers.image.title="Your Image Title"
LABEL org.opencontainers.image.description="Description of your image"
LABEL org.opencontainers.image.logo="https://url-to-your-logo.png"
# Add Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Install required dependencies and tools
RUN apt-get update && apt-get install -y \
    wget \
    vim \
    less \
    mariadb-client \
    git \
    unzip \
    libzip-dev \
    && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install \
    pdo_mysql \
    opcache \
    mysqli \
    zip

# Install and configure the PHP Redis extension
RUN pecl install redis \
    && docker-php-ext-enable redis

# Install php-fpm-healthcheck
RUN wget -O /usr/local/bin/php-fpm-healthcheck \
    https://raw.githubusercontent.com/renatomefi/php-fpm-healthcheck/master/php-fpm-healthcheck \
    && chmod +x /usr/local/bin/php-fpm-healthcheck

# Configure PHP-FPM for healthcheck
RUN set -xe && \
    echo "pm.status_path = /status" >> /usr/local/etc/php-fpm.d/zz-docker.conf

# Configure PHP for WordPress
RUN { \
    echo 'memory_limit = 256M'; \
    echo 'upload_max_filesize = 64M'; \
    echo 'post_max_size = 64M'; \
    echo 'max_execution_time = 300'; \
    echo 'max_input_vars = 3000'; \
    } > /usr/local/etc/php/conf.d/wordpress.ini

# Configure OPcache
RUN { \
    echo 'opcache.memory_consumption=128'; \
    echo 'opcache.interned_strings_buffer=8'; \
    echo 'opcache.max_accelerated_files=4000'; \
    echo 'opcache.revalidate_freq=2'; \
    echo 'opcache.fast_shutdown=1'; \
    } > /usr/local/etc/php/conf.d/opcache-recommended.ini

# Set working directory
WORKDIR /var/www/html

# Install Bedrock WordPress
RUN set -ex; \
    composer create-project roots/bedrock . --no-dev; \
    composer require wpackagist-plugin/redis-cache; \
    composer require wpackagist-plugin/wordpress-seo; \
    composer require wpackagist-plugin/wp-mail-smtp; \
    rm -rf /var/www/html/web/app/uploads; \
    mkdir -p /var/www/html/web/app/uploads

# Set proper permissions
RUN chown -R www-data:www-data /var/www/html

# Switch to non-root user
USER www-data

# Healthcheck configuration
HEALTHCHECK --interval=10s --timeout=3s --start-period=30s --retries=3 \
    CMD php-fpm-healthcheck || exit 1

# Default command
CMD ["php-fpm"]
