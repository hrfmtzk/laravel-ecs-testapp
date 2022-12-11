FROM php:8-fpm

# Set environment variables
ENV COMPOSER_ALLOW_SUPERUSER=1 \
    # system environment
    TZ=Asia/Tokyo \
    # php.ini variables
    PHP_DATE_TIMEZONE=Asia/Tokyo \
    PHP_DISPLAY_ERRORS=0 \
    PHP_MEMORY_LIMIT=512M \
    PHP_MAX_EXECUTION_TIME=300 \
    PHP_POST_MAX_SIZE=50M \
    PHP_UPLOAD_MAX_FILESIZE=50M \
    # application variables
    APP_NAME=testapp \
    APP_ENV=local \
    APP_KEY="" \
    APP_DEBUG=false \
    APP_URL=http://localhost \
    LOG_CHANNEL=stderr \
    LOG_LEVEL=info \
    DB_CONNECTION=mysql \
    DB_HOST=localhost \
    DB_PORT=3306 \
    DB_DATABASE=testapp \
    DB_USERNAME=root \
    DB_PASSWORD=password

# Copy composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Install system dependencies
RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
        locales \
        git \
        unzip \
        libzip-dev \
        libicu-dev \
        libonig-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install \
    intl \
    pdo_mysql \
    zip \
    bcmath

# Use php production configuration
RUN ln -s /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini

# Copy php application configuration file
COPY ./ecs_docker/app/php.app.ini /usr/local/etc/php/conf.d/php.app.ini

# Set working directory
WORKDIR /app

# Copy application code
COPY . /app

# Setup application
RUN composer install --optimize-autoloader --no-dev \
    && chown www-data:www-data -R storage

CMD [ "bash", "-c", "php artisan config:cache && php artisan route:cache && php artisan view:cache && php-fpm" ]
