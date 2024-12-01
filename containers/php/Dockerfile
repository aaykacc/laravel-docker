FROM php:8.2-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libonig-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    git \
    curl \
    libmemcached-dev \
    zlib1g-dev \
    libmcrypt-dev \
    libreadline-dev \
    libgmp-dev \
    libzip-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    openssh-server \
    libmagickwand-dev \
    cron \
    nano \
    wget \
    unzip \
    psmisc \
    libxml2-dev \
    libxslt-dev

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install \
    pdo_mysql \
    mbstring \
    exif \
    pcntl \                                                                                                                                                                                                                                                                                                  1,1           Top
    bcmath \                                                                                                                                                                                                                                                                                                 107,16        Bot
    gd \
    mysqli \
    pdo \
    pdo_mysql \
    xml \
    pcntl \
    bcmath \
    pdo_pgsql \
    zip \
    intl \
    gettext \
    soap \
    sockets \
    calendar \
    exif

# Install Node.js and npm
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g npm@latest


# Change home path and UID/GID of www-data user
RUN usermod -d /home/www-data -u 1000 www-data && \
    groupmod -g 1000 www-data

# Change npm cache folder to avoid permission issues
RUN mkdir -p /home/www-data/.npm && npm config set cache /home/www-data/.npm --global
RUN chown -R www-data:www-data /home/www-data/.npm

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set working directory
WORKDIR /var/www

# Set up aliases and history control for all users
RUN echo 'alias pa="php artisan"' >> /etc/profile.d/common.sh && \
    echo 'alias tink="php artisan tinker"' >> /etc/profile.d/common.sh && \
    echo 'HISTCONTROL=erasedups' >> /etc/profile.d/common.sh && \
    echo "PROMPT_COMMAND='history -w'" >> /etc/profile.d/common.sh && \
    chmod +x /etc/profile.d/common.sh && \
    echo 'source /etc/profile.d/common.sh' >> /etc/bash.bashrc && \
    echo 'source /etc/profile.d/common.sh' >> /etc/bash.bashrc

# Copy existing application directory contents
COPY --chown=www-data:www-data . .

# Install dependencies
RUN composer install --optimize-autoloader --no-dev

# Set permissions
RUN chown -R www-data:www-data /var/www/storage /var/www/bootstrap/cache

# Switch to www-data user
USER www-data

# Expose port 9000 and start php-fpm server
EXPOSE 9000

CMD ["php-fpm"]