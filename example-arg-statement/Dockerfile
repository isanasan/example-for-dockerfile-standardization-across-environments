# syntax=docker/dockerfile:1.4
FROM php:8.2-fpm-alpine

ARG ENVIRONMENT
ENV ENVIRONMENT ${ENVIRONMENT:-dev}

# PHP設定ファイル
COPY --link ${ENVIRONMENT}/php.ini /usr/local/etc/php/