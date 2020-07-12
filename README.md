# ansible essentials learning course files

## prepare the environment

### requirements

* python 2.7+ or 3+
* virtualenv
* docker

### expected hosts

The following hosts are expected to be accessible via ssh+keypair

* remote host:web1 ip:127.0.0.1 port:2220 ssh_user: root
* remote host:web2 ip:127.0.0.1 port:2221 ssh_user: root
* remote host:db1 ip:127.0.0.1 port:2222 ssh_user: root
* remote host:db2 ip:127.0.0.1 port:2223 ssh_user: root

### /etc/hosts

Add the following hosts to the file /etc/hosts

```
$ cat /etc/hosts
...
127.0.0.1 db1
127.0.0.1 web2
127.0.0.1 web1
127.0.0.1 db2
```

### create ssh keys

```
$ ssh-keygen -t rsa
```

[more details](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2)

### virtualenv

```
$ virtualenv .venv
$ source .venv/bin/activate
$ pip install -r requirements.txt
```

### use docker to setup expected remote hosts

```
docker build -t kartoza/ssh git://github.com/timlinux/docker-ssh
docker run --name web1 -p 2220:22 -d -t kartoza/ssh
docker logs web1 | grep 'root login password'
ssh root@localhost -p 2220
ssh-copy-id root@localhost -p 2220
```

## Experiments

### first experiment

#### test
```
ansible-playbook -i hosts tasks/2-1-tasks.yml -e file_state=touch
```

#### result
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
### second experiment

#### test

```
  ansible-playbook -i hosts tasks/2-2-tasks.yml --tags create-file
  ansible-playbook -i hosts tasks/2-2-tasks.yml --tags delete-file
```
