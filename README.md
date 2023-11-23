# devops-a2-part1

# NGINX Container

```console
version: '3'

services:
  web_nginx:
    image: nginx
    container_name: nginx_container
    networks:
      - my_network
    ports:
      - "8080:80"
networks:
  my_network:
    driver: bridge
```

# Description

```console
COMMAND: docker-compose up -d
```

```console
Created a container using nginx image and custom bridge network. then connected container with bridge network and exposed port 80 with host port 8080. tested by running
```

# HTTPD container

```console
services:
  web_nginx:
    image: nginx
    container_name: nginx_container
    networks:
      - my_network
    ports:
      - "8080:80"
  web_httpd:
    image: httpd
    container_name: httpd_container
    networks:
      - my_network
    ports:
      - 8081:80

networks:
  my_network:
    driver: bridge
```

# Description

```console
Created another conatiner in docker compose file, connected it with my_network but used host port 8081 for httpd. at the end verified that http://localhost:8081 is wokring and showing It works!
```

# docker-compose up -d

```console
[+] Building 0.0s (0/0)                                                                                                                                                                                  docker:desktop-linux
[+] Running 2/0
 ✔ Container httpd_container  Running                                                                                                                                                                                    0.0s
 ✔ Container nginx_container  Running                                                                                                                                                                                    0.0s
```

# docker network inspect my_network

```console
[
    {
        "Name": "my_network",
        "Id": "2d98368b74619dcbb120e5e4373d639a48a8d830186e621a0b426fc04ffa89ec",
        "Created": "2023-11-22T15:28:35.41098076Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

# Stop nginx container command

```console
docker-compose stop web_nginx
```

# Output

```console
[+] Stopping 1/1
 ✔ Container nginx_container  Stopped
```

# remove nginx container

```console
docker-compose rm web_nginx
```

# output

```console
? Going to remove nginx_container Yes
[+] Removing 1/0
 ✔ Container nginx_container  Removed
```

# Create another container using nginx image

```console
version: '3'

services:
  web_nginx_2:
    image: nginx
    container_name: nginx_container_2
    networks:
      - my_network
    ports:
      - "8082:80"
  web_httpd:
    image: httpd
    container_name: httpd_container
    networks:
      - my_network
    ports:
      - 8081:80

networks:
  my_network:
    driver: bridge
```

# docker-compose up -d

```console
[+] Building 0.0s (0/0)                                                                                                                        docker:desktop-linux
[+] Running 2/2
 ✔ Container nginx_container_2  Started                                                                                                                        0.0s
 ✔ Container httpd_container    Running
```

# http://localhost:8082/ is working fine

# command - docker container ls

```console
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                  NAMES
32f60e154b16   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   0.0.0.0:8082->80/tcp   nginx_container_2
886792a136fe   httpd     "httpd-foreground"       2 hours ago     Up 8 minutes   0.0.0.0:8081->80/tcp   httpd_container
```

# docker-compose stop

```console
[+] Stopping 2/2
 ✔ Container httpd_container    Stopped                                                                                                                        1.2s
 ✔ Container nginx_container_2  Stopped
```

# docker-compose rm

```console
? Going to remove nginx_container_2, httpd_container Yes
[+] Removing 2/0
 ✔ Container httpd_container    Removed                                                                                                                        0.0s
 ✔ Container nginx_container_2  Removed
```

# remove my_network

```console
docker network rm my_network
```
