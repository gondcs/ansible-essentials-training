# setup docker instances as remote hosts

````
docker build -t kartoza/ssh git://github.com/timlinux/docker-ssh
docker run --name "web1" -p 2220:22 -d -t kartoza/ssh
docker logs web1 | grep 'root login password'
ssh root@localhost -p 2220
ssh-copy-id root@localhost -p 2220
```

## experiment 02 01

```
ssh root@web1 -p 2220
file  web-file  web-not-db-file
ssh root@web2 -p 2221
file  web-file  web-not-db-file
ssh root@db1 -p 2222
file
ssh root@db2 -p 2223
backup-file file
```
# ll-ansible-essentials
