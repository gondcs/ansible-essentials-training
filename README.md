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

### third experiment

#### test

```
$ ansible-playbook -i hosts tasks/2-3-tasks.yml 
```

#### result
```
PLAY [localhost] ****************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************
ok: [localhost]

TASK [create a file via ssh connection] *****************************************************************************
changed: [localhost]

PLAY [localhost] ****************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************
ok: [localhost]

TASK [create a file via direct local connection] ********************************************************************
changed: [localhost]

PLAY RECAP **********************************************************************************************************
localhost                  : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

$ ls /tmp/*-created
/tmp/direct-created	/tmp/ssh-created
```
### forth experiment

#### test

```
ap -i hosts tasks/2-4-tasks.yml -e file_state=touch
ap -i hosts tasks/2-4-tasks.yml -e file_state=absent
ap -i hosts tasks/2-4-tasks.yml -e file_state=touch --start-at-task='the second task'
```

### fifth experiment

#### test

```
ap -i 2-5-inventory tasks/2-5-tasks.yml -e file_state=touch
ap -i 2-5-inventory tasks/2-5-tasks.yml -e file_state=absent
```

### eighth experiment

#### test

```
ap -i 2-5-inventory tasks/2-8-tasks.yml --tags create
```
### ninth experiment

#### test

```
ap -i 2-5-inventory tasks/2-9-tasks.yml -e file_state=touch
```

### tenth experiment

#### test

```
ap -i 2-5-inventory tasks/2-10-tasks.yml -e pkg_state=latest -e file_state=touch
```

### eleventh experiment

#### test

```
ap -i 2-5-inventory tasks/2-11-tasks.yml -e file_state=touch
ap -i 2-5-inventory tasks/2-11-tasks.yml -e file_state=absent
```

### twelveth experiment

#### test

```
ap -i 2-5-inventory tasks/2-12-tasks.yml
```

### thirteenth experiment

#### test

```
ap -i 2-5-inventory tasks/2-13-tasks.yml --tags create
ap -i 2-5-inventory tasks/2-13-tasks.yml --tags destroy
```
### fourteenth experiment

#### test

```
ap -i 2-5-inventory tasks/2-13-tasks.yml --tags create --check
```

#### expected

* There should be no /tmp/2-13-template.txt created in any of the web1, web2, db1 and db2 hosts
* Ansible should finish succesfully
* Ansible should create temporary files in all the remote hosts (/tmp/ansible_setup_payload_*)
