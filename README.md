## Kontenery Docker na serwerze Sigma

Agenda
Docker overview -  https://docs.docker.com/engine/docker-overview/#the-docker-platform
Docker-machine overview - https://docs.docker.com/machine/overview/
Docker-compose overview - https://docs.docker.com/compose/overview/
Docker example for Rails development - https://docs.docker.com/compose/rails/#define-the-project

Przydatne linki: 
    https://www.codeschool.com/courses/try-docker

Docker machine:
export MACHINE_STORAGE_PATH=/tmp/st/.docker
docker-machine create default
docker-machine env
eval $(docker-machine env)

Create Dockerfile:
FROM ruby:2.5.0
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
RUN mkdir /myapp
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN bundle install
COPY . /myapp

Create Gemfile:
source 'https://rubygems.org'
gem 'rails', '= 5.1.5'

Touch Gemfile.lock:
touch Gemfile.lock

Create docker-compose.yml:
version: '3'
services:
  db:
    image: postgres
  web:
    build: .
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db

Build docker images:
docker-compose build

Rails configuration

Replace content `config/database.yml`:
default: &default
  adapter: postgresql
  host: db
  username: postgres
  password:
  pool: 5

development:
  <<: *default
  database: myapp_development


test:
  <<: *default
  database: myapp_test

Run:
docker-compose up

Create DB:
docker-compose run web rake db:create

Pushing Docker image to Docker Cloud

Push docker image to Docker could:
export DOCKER_ID_USER="username"
docker tag my_image $DOCKER_ID_USER/my_image
docker push $DOCKER_ID_USER/my_image

mongo:
https://hub.docker.com/_/mongo/

