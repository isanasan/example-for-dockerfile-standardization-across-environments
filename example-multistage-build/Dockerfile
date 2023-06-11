# syntax=docker/dockerfile:1.4
FROM php:8.2-fpm-alpine as build

# ここで色々インストールする

FROM build as dev

# composer本体を持ってくる
COPY --link --from=composer:latest /usr/bin/composer /usr/bin/

# 開発環境だけxdebugを入れる
RUN <<EOF
    apk add gcc g++ make autoconf linux-headers
    pecl install xdebug-3.2.1
    docker-php-ext-enable xdebug
EOF

# 本番環境用に依存パッケージをインストールする
FROM composer:latest as dependency

COPY composer.* /app/

RUN composer install \
    --no-dev \
    --ignore-platform-reqs \
    --no-interaction \
    --prefer-dist \
    --no-plugins \
    --no-scripts \
    --no-autoloader

COPY --link ./ /app/

RUN composer dump-autoload \
    --no-dev \
    --optimize

FROM build as prd

# 依存バッケージを含むアプリケーションに必要なファイル郡を持ってくる
COPY --link --from=dependency /app/ /var/www/
