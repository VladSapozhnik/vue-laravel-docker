FROM php:7.3-fpm

USER root

# Set working directory
WORKDIR /var/www

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libzip-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    libc6 \
    libfreetype6 \
    libgd3 \
    libwebp-dev \
    libxpm-dev \
    libfreetype6-dev \
    zlib1g \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl gd
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-webp-dir=/usr/include/  --with-jpeg-dir=/usr/include/
RUN docker-php-ext-enable gd

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install nodejs
#ENV NODE_VERSION=12.6.0
#RUN apt install -y curl
#RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
#ENV NVM_DIR=/root/.nvm
#RUN . "$NVM_DIR/nvm.sh" && nvm install ${NODE_VERSION}
#RUN . "$NVM_DIR/nvm.sh" && nvm use v${NODE_VERSION}
#RUN . "$NVM_DIR/nvm.sh" && nvm alias default v${NODE_VERSION}
#ENV PATH="/root/.nvm/versions/node/v${NODE_VERSION}/bin/:${PATH}"
#RUN node --version
#RUN npm --version
RUN apt-get install -y curl
RUN rm -rf /var/lib/apt/lists/ && curl -sL https://deb.nodesource.com/setup_12.x | bash
RUN apt-get install nodejs -y
# Cleaning
RUN apt-get clean && apt-get autoremove -y

#RUN rm /etc/apt/preferences.d/no-debian-php && apt-get update -yqq && apt-get -y install libxml2-dev php-soap && docker-php-ext-install soap

RUN rm -rf /var/lib/apt/lists/*

# Copy existing application directory contents
COPY ./src /var/www

# Copy existing application directory permissions

# Change current user to www-data
USER www-data

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]