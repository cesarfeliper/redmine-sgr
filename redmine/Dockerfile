FROM ruby:2.6-alpine
WORKDIR /usr/src/app

MAINTAINER YoshinoriN

RUN apk update --no-cache \
  && apk add --no-cache --virtual build-dependencies libffi-dev build-base \
  && apk add --no-cache mysql-client tzdata mariadb-dev=10.4.13-r0 linux-headers=5.4.5-r1 imagemagick-dev \
  && apk add --no-cache git

RUN mkdir  /usr/src/app/redmine
COPY ./src /usr/src/app/redmine
COPY Gemfile /usr/src/app/redmine/Gemfile
COPY docker-entrypoint.sh /usr/src/app/redmine/docker-entrypoint.sh
RUN chmod +x /usr/src/app/redmine/docker-entrypoint.sh

WORKDIR /usr/src/app/redmine

RUN gem install bundler -v 2.1.4 --no-document \
  && bundle install --jobs=10 --without development test \
  && bundle exec rake generate_secret_token \
  && apk del --purge build-dependencies \
  && rm -rf /var/cache/apk/*

ENTRYPOINT ["/usr/src/app/redmine/docker-entrypoint.sh"]

EXPOSE 3000
