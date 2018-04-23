FROM ruby:2.5.0
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs

RUN gem install rails pry-rails pg
RUN rails new my_app --database=postgresql --skip-action-cable --skip-coffee --skip-test --skip-bundle

WORKDIR /my_app

RUN bin/bundle install
