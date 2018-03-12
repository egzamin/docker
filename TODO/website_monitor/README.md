## Using volumes with NoSQL databases!

```sh
host_dir=~/Repos/egzamin/docker/website_monitor

docker run -d --name bmweb \
    -v $host_dir:/usr/local/apache2/htdocs \
    -p 80:80 \
    httpd:latest

docker rm -vf bmweb

docker run --name bmweb_ro \
    --volume $host_dir:/usr/local/apache2/htdocs/:ro \
    -p 80:80 \
    httpd:latest
```

```sh
host_dir=~/Repos/egzamin/docker/website_monitor
mkdir -p $host_dir

docker run --name plath -d \
    -v $host_dir:/data \
    dockerinaction/ch4_writer_a		

docker run --rm \
    -v $host_dir:/reader-data \
    alpine:latest \
    head /reader-data/logA		

cat $host_dir/logA

docker stop plath

docker rm -v $(docker ps -aq)
```



## TODO

```sh
docker run -d \
    --volume /var/lib/cassandra/data \
    --name cass-shared \
    alpine echo Data Container

docker run -d \
    --volumes-from cass-shared \
    --name cass2 \
    cassandra:2.2
```


```sh
docker run --link cass2:cass cassandra:2.2 -it --rm cqlsh cass
```

```sql
select *
from system.schema_keyspaces
where keyspace_name = 'docker_hello_world';
```
