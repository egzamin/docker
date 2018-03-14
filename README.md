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

Po zalogowaniu się na swoje konto, polecenie:
```sh
echo $MACHINE_STORAGE_PATH
```
powinno wypisać na terminalu napis podobny do `/tmp/xxx/.docker`.
Oczywiście, można zawsze samemu wyeksportować zmienną `MACHINE_STORAGE_PATH`:
```
export MACHINE_STORAGE_PATH=/tmp/abc/.docker
```

Zanim zaczniemy ściągać obrazy, tworzyć kontenery itd. musimy wykonać te
polecenia (albo dopisać je do pliku startowego powłoki Bash):
```sh
docker-machine create default
docker-machine env # sprawdzamy jakie wartości mają zmienne z których korzystają klienci Dockera
eval $(docker-machine env) # zapisujemy je w środowisku
```
**Uwaga** Wykonanie pierwszego polecenia zajmuje ok 1-3 minut (w lab. 109).

Na początek, można ściągnąć kilka obrazów z [Docker Hub](https://hub.docker.com/):
```sh
docker pull alpine:latest
docker pull nginx:latest
```
To polecenie pokaże jak duże są te obrazy
```sh
docker images
# nginx   latest  e548f1a579cf  3 weeks ago   109.00MB
# alpine  latest  3fd9065eaf02  2 months ago    4.15MB
```
Niektórych może zdziwić, że kompletny dystrybucja linux „Alpine Linux”
może zająć ok. 5 MB!

Jeśli coś poszło nie tak to zawsze można zacząć od początku usuwając maszynę
`default`:
```sh
docker-machine rm default
```


## Kontynuacja dla „Technologie NoSQL”

* [Docker Volumes](https://github.com/egzamin/nosql/blob/master/docker/volumes.adoc).


## Kontynuacja dla „Architektura serwisów internetowych”

* [Docker Compose](https://github.com/egzamin/asi/blob/master/docker/docker_compose.adoc).


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
