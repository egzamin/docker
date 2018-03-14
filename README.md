## Kontenery Docker na komputerach w laboratoriach

Kurs na [Code School](https://www.codeschool.com/):

* [Try Docker](https://www.codeschool.com/courses/try-docker)

Official images on Docker Hub:

* [Ruby](https://hub.docker.com/_/ruby/)
* [Mongo](https://hub.docker.com/_/mongo/)

Bitnami images:

* [Rails](https://hub.docker.com/r/bitnami/rails/)
* [Ruby](https://hub.docker.com/r/bitnami/ruby/)
* [MongoDB](https://hub.docker.com/r/bitnami/mongodb/)

Dokumentacja, samouczki, przykłady:

* [Docker overview](https://docs.docker.com/engine/docker-overview/)
* [Get started with Docker](https://docs.docker.com/get-started/)
* [Docker-machine overview](https://docs.docker.com/machine/overview/)
* [Docker-compose overview](https://docs.docker.com/compose/overview/)
* [Samples](https://docs.docker.com/samples/):
  * [Docker example for Rails development](https://docs.docker.com/compose/rails/)
  * [Mongo](https://docs.docker.com/samples/library/mongo/)


## Docker Machine w laboratoriach

Po zalogowaniu się na swoje konto, polecenie
```sh
echo $MACHINE_STORAGE_PATH
```
powinno wypisać na terminalu napis podobny do `/tmp/xxx/.docker`

Następnie wykonujemy te polecenia:
```sh
# Możemy samemu wyekportować zmienną MACHINE_STORAGE_PATH
# export MACHINE_STORAGE_PATH=/tmp/<your login>/.docker
docker-machine create default
docker-machine env # sprawdzamy
eval $(docker-machine env)
```
Wykonanie wszystkich poleceń zajmuje ok 1-3 minut.

Teraz możemy ściągnąć kilka obrazów z [Docker Hub](https://hub.docker.com/):
```sh
docker pull alpine:latest
docker pull nginx:latest
```

Jeśli coś poszło nie tak to możemy zacząć jeszcze raz usuwając
maszynę `default`:
```sh
docker-machine rm default
```


## Docker Compose

**TODO**

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
gem 'rails', '= 5.2.0.rc1'
```

Touch _Gemfile.lock_:
```sh
touch Gemfile.lock
```

## Docker Compose

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
docker-compose run web rails new . --force --database=postgresql
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

Create Postgres database:
```sh
docker-compose run web rails db:create
```

Finally, push docker image to Docker Hub:

```sh
docker image ls --all

export DOCKER_ID_USER="username"
docker tag my_image $DOCKER_ID_USER/my_image
docker push $DOCKER_ID_USER/my_image
```


## Local installation of Docker Machine & Compose

[Docker Machine](https://docs.docker.com/machine/install-machine/).
```sh
curl -L https://github.com/docker/machine/releases/download/v0.14.0/docker-machine-`uname -s`-`uname -m` \
> /tmp/docker-machine && install /tmp/docker-machine $HOME/bin/docker-machine
docker-machine version
```

Install Bash completions for the _docker-machine_ program.
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
