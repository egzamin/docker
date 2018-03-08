## Kontenery Docker na serwerze Sigma

* Docker overview -  https://docs.docker.com/engine/docker-overview/#the-docker-platform
* Docker-machine overview - https://docs.docker.com/machine/overview/
* Docker-compose overview - https://docs.docker.com/compose/overview/
* Docker example for Rails development - https://docs.docker.com/compose/rails/#define-the-project

Przydatne linki:

* https://www.codeschool.com/courses/try-docker

Docker machine:

```sh
export MACHINE_STORAGE_PATH=/tmp/st/.docker
docker-machine create default
docker-machine env
eval $(docker-machine env)
```

Create _Dockerfile_:

```sh
FROM ruby:2.5.0
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs
RUN mkdir /myapp
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN bundle install
COPY . /myapp
```

Create _Gemfile_:

```ruby
source 'https://rubygems.org'
gem 'rails', '= 5.2.0'
```

Touch _Gemfile.lock_:

```sh
touch Gemfile.lock
```

Create _docker-compose.yml_:

```yaml
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
```

Build docker images:

```sh
docker-compose build
```

Rails configuration:

* Replace content `config/database.yml`:

```yaml
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
```

Run:

```sh
docker-compose up
```

Create DB:

```sh
docker-compose run web rake db:create
```

Push docker image to Docker could:

```sh
export DOCKER_ID_USER="username"
docker tag my_image $DOCKER_ID_USER/my_image
docker push $DOCKER_ID_USER/my_image
```

[Mongo](https://hub.docker.com/_/mongo/).
