Docker-compose

```
$ docker-compose up [-d]
```

```
$ docker-compose logs
```

Build once again with no cache

```
$ docker-compose build --no-cache
```

Commands similar to docker

```
$ docker-compose ps
$ docker-compose top
$ docker-compose logs -f [service_name]
$ docker-compose {start|restart|stop|kill}
$ docker-compose {up|down}
$ docker-compose exec SERVICE_NAME ping 8.8.8.8
$ docker network ls
$ docker volume ls
```

```
$ docker-compose run SERVICE_NAME bash
```

```
$ docker-compose {pull|push}
```