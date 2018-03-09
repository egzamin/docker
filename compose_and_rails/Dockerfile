FROM ruby:2.5.0
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
RUN mkdir /my_classrooms
WORKDIR /my_classrooms
COPY Gemfile /my_classrooms/Gemfile
COPY Gemfile.lock /my_classrooms/Gemfile.lock
RUN bundle install
COPY . /my_classrooms
