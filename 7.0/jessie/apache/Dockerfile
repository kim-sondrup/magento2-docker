#
# NOTE: THIS DOCKERFILE IS GENERATED VIA "update.sh"
#
# PLEASE DO NOT EDIT IT DIRECTLY.
#

FROM php:7.0-apache-jessie

RUN set -xe \
    && provides () { apt-cache showpkg $1 | awk '/Pa/, /Reverse P/ {next} {print $1 | "sort"}' | uniq;} \
    && search_deps () { apt-cache depends $1 | awk '$1 == "Depends:" && $2 ~ "'"$2"'" {print $2 | "sort"}' | uniq | head -n 1;} \
    && apt-get update \
    && libDeps=" \
        libfreetype6 \
        $(search_deps 'libicu-dev' '^libicu') \
        $(search_deps "$(provides libpng-dev)" '^libpng') \
        libjpeg62-turbo \
        libmcrypt4 \
        $(search_deps 'libxslt1-dev' '^libxslt') \
    " \
    && apt-get install -y $libDeps --no-install-recommends && rm -r /var/lib/apt/lists/*

##<autogenerated>##
RUN set -ex \
# setup directories and permissions
	&& . "$APACHE_ENVVARS" \
	&& for dir in \
		/var/www/magento2 \
	; do \
		rm -rvf "$dir" \
		&& mkdir -p "$dir" \
		&& chown -R "$APACHE_RUN_USER:$APACHE_RUN_GROUP" "$dir"; \
	done

# Replace path "/var/www/html" with "/var/www/magento2"
RUN sed -ir \
		-e 's!/var/www/html!/var/www/magento2!g' \
		 "$APACHE_CONFDIR"/sites-available/*.conf

# PHP files should be handled by PHP, and should be preferred over any other file type
RUN { \
		echo '<Directory "/var/www/magento2">'; \
		echo '\tOptions Indexes FollowSymLinks MultiViews'; \
		echo '\tAllowOverride All'; \
		echo '</Directory>'; \
	} | tee "$APACHE_CONFDIR/conf-available/magento2.conf" \
	&& a2enconf magento2
##</autogenerated>##

RUN set -xe \
    && buildDeps=" \
        libfreetype6-dev \
        libicu-dev \
        libpng-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libxslt1-dev \
    " \
    && apt-get update && apt-get install -y $buildDeps --no-install-recommends && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
    && docker-php-ext-install \
        bcmath \
        dom \
        gd \
        intl \
        mcrypt \
        pdo_mysql \
        xsl \
        zip \
        soap \
    && docker-php-source delete \
    \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false $buildDeps \
    \
    && pecl update-channels \
    && rm -rf /tmp/pear ~/.pearrc \
    && true # Done

##<autogenerated>##
WORKDIR /var/www/magento2
##</autogenerated>##
