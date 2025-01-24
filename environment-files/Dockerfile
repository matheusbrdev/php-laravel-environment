FROM php:8.3-fpm

ARG user
ARG node
ARG uid=1000

# Install system dependencies
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    vim \
    autoconf \
    apt-transport-https \
    apt-utils \
    file \
    git \
    gnupg2 \
    libbz2-dev \
    libjpeg-dev \
    libltdl-dev \
    libmcrypt-dev \
    libonig-dev \
    libpng-dev \
    libmagickcore-dev \
    libmagickwand-dev \
    libpng-dev \
    libsodium-dev \
    libtiff-dev \
    libtool \
    libxml2 \
    libxml2-dev \
    libzip-dev \
    openssl \
    procps \
    rsyslog \
    software-properties-common \
    unzip \
    unixodbc \
    unixodbc-dev \
    wget \
    zip

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN apt-get update \ 
    && apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql mbstring xml exif pcntl bcmath gd \
    sodium zip bcmath soap opcache

# Get latest Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# NodeJS
RUN curl -fsSL https://deb.nodesource.com/setup_$node.x | bash - \
    && apt-get update \ 
    && apt-get install -y nodejs \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Create system user to run Composer and Artisan Commands
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

# Set BR timezone
RUN echo America/Sao_Paulo > /etc/timezone \
    && export TZ=America/Sao_Paulo

# Set working directory
WORKDIR /var/www/html

USER $user