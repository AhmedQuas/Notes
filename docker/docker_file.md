Specify base image

```
FROM ubuntu
```

```
RUN apt update && apt install -y \
wget curl

RUN sed -i "s/old_line/new_line/" file_path
```

Set workdir

```
WORKDIR /path_to_workdir
```

Copy content between container and host OS

```
COPY website /var/www/html
ADD website /var/www/html
```

```
WORKDIR /var/www/html
COPY . . 
```

ADD can also use urls


Default command to run in container

Only the last one will be executed. Running container in interactive mode will overwite CMD specified in Dockerfile.
 
```
CMD ["curl", "-I", "http://example.com"]
```

Works similar to CMD but in ENTRYPOINT we can pass args.

```
ENTRYPOINT ["curl","-I"] 
```

```
$ docker run IMAGE_NAME ARG
```

Override ENTYPOINT

```
$ docker run -it --entrypoint bash IMAGE_NAME
```

Allow container to be reachable in specified port

```
EXPOSE 80
```

