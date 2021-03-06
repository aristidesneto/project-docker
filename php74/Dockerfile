FROM php:7.4-fpm

LABEL maintainer="Aristides Neto"

RUN useradd -ms /bin/bash -u 1000 development
RUN usermod -aG root development

ENV DEBIAN_FRONTEND noninteractive

ENV TZ=America/Sao_Paulo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update --fix-missing \
    && apt-get install -y --no-install-recommends \
    ssh \
    sudo \
    locales \
    curl \
    vim \
    ca-certificates \
    zip \
    unzip \
    git \
    sqlite3 \
    libbz2-dev \
    libxslt-dev \
    libzip-dev \
    zlib1g-dev \
    libcap2-bin \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    python2 \
    nginx \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
   
# Install composer
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer

# Install node/npm
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - 
RUN apt-get install -y nodejs

# Install deployer
RUN curl -LO https://deployer.org/deployer.phar
RUN mv deployer.phar /usr/local/bin/dep
RUN chmod +x /usr/local/bin/dep

RUN docker-php-ext-install bcmath bz2 pdo_mysql zip exif

RUN docker-php-ext-configure gd --enable-gd --with-jpeg
RUN docker-php-ext-install gd

COPY nginx.conf /etc/nginx/conf.d/app.conf

EXPOSE 80

WORKDIR /var/www/html

CMD ["nginx", "-g", "daemon off;"]
