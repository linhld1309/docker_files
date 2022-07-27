# fetch node
FROM --platform=linux/x86_64 node:16-alpine as node

FROM --platform=linux/x86_64 ruby:3.1-alpine
ENV LANG C.UTF-8
ENV BUNDLE_JOBS 4
ENV BUNDLE_PATH=/bundle

COPY --from=node /usr/local/bin/node /usr/local/bin/node

RUN apk update \
  && apk add --update --no-cache \
    bash \
    git \
    build-base \
    curl \
    tzdata \
    mariadb-client \
    mariadb-dev \
    mariadb-connector-c \
    libc6-compat \
    libssl1.1 \
    openssl \
    rsync \
    sudo \
    shared-mime-info \
    less \
  && rm -rf /var/lib/apt/lists/*

RUN gem update

WORKDIR /app
EXPOSE 3000
