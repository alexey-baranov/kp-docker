# stack
```
$ docker stack deploy -c stack.yaml kopnik
$ docker stack ps kopnik
$ docker container ps
$ docker stack rm kopnik
```

### подключиться к работающему контейнеру
https://askubuntu.com/questions/505506/how-to-get-bash-or-ssh-into-a-running-container-in-background-mode
```
sudo docker exec -i -t 6fee /bin/bash
```

### кроссбарный образ
```
docker build --no-cache --tag kp-crossbar .
```


