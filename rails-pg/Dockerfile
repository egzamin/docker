FROM ruby:2.5.0
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs

RUN gem install rails --version '~> 5.2.0'
RUN rails new myapp --database=postgresql --skip-action-cable --skip-coffee --skip-test --skip-bundle

WORKDIR /myapp

COPY database.yml config

RUN bin/bundle install

COPY Dockerfile .
COPY docker-compose.yml .
