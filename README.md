## Kontenery Docker na komputerach w laboratoriach

* [Docker overview](https://docs.docker.com/engine/docker-overview/)
* [Docker-machine overview](https://docs.docker.com/machine/overview/)
* [Docker-compose overview](https://docs.docker.com/compose/overview/)
* [Samples](https://docs.docker.com/samples/):
  * [Docker example for Rails development](https://docs.docker.com/compose/rails/)
  * [Mongo](https://docs.docker.com/samples/library/mongo/);
    [zob. też](https://hub.docker.com/_/mongo/).

Przydatne linki:

* https://www.codeschool.com/courses/try-docker

Docker machine:

```sh
export MACHINE_STORAGE_PATH=/tmp/<unikalne id>/.docker
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
docker-compose run web rails db:create
```

Push docker image to Docker Hub:

```sh
export DOCKER_ID_USER="username"
docker tag my_image $DOCKER_ID_USER/my_image
docker push $DOCKER_ID_USER/my_image
```


## Machine & Compose – local installations

[Docker Machine](https://docs.docker.com/machine/install-machine/).

```sh
curl -L https://github.com/docker/machine/releases/download/v0.14.0/docker-machine-`uname -s`-`uname -m` \
> /tmp/docker-machine && install /tmp/docker-machine $HOME/bin/docker-machine
docker-machine version
```

```sh
mkdir -p $HOME/.bash_completion.d
scripts=(docker-machine-prompt.bash docker-machine-wrapper.bash docker-machine.bash)
for i in "${scripts[@]}"; do
  wget https://raw.githubusercontent.com/docker/machine/v0.14.0/contrib/completion/bash/${i} \
      -P $HOME/.bash_completion.d
done
```

[Docker Compose](https://docs.docker.com/compose/install/).

```sh
curl -L https://github.com/docker/compose/releases/download/1.19.0/docker-compose-`uname -s`-`uname -m` \
  -o $HOME/bin/docker-compose
chmod 755 $HOME/bin/docker-compose
docker-compose version
```
