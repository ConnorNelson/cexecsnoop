# cexecsnoop

Like [execsnoop](https://github.com/iovisor/bpftrace/blob/master/tools/execsnoop.bt), but isolated to a specified container.

## Install

You need to install bpftrace.
See bpftrace's [INSTALL.md](https://github.com/iovisor/bpftrace/blob/master/INSTALL.md).

## Usage

```sh
./cexecsnoop.bt <container_pid>
```

It can easily be run for a particular running docker container with:

```bash
./cexecsnoop.bt $(docker inspect --format '{{.State.Pid}}' <container_name|container_id>)
```

## Example

```sh
$ docker run -it --rm --name=example ubuntu:20.04
root@1450d7b93e31:/# whoami
root
root@1450d7b93e31:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  etc  lib   lib64  media   opt  root  sbin  sys  usr
root@1450d7b93e31:/# sh
# id
uid=0(root) gid=0(root) groups=0(root)
```

```sh
# ./cexecsnoop.bt $(docker inspect --format '{{.State.Pid}}' example)
Attaching 5 probes...
TIME(ms)   PPID       PID        PATHNAME             ARGV
7085       488509     488997     /usr/bin/whoami      whoami
12318      488509     489006     /usr/bin/ls          ls, --color=auto
20130      488509     489106     /usr/bin/sh          sh
27319      489106     489200     /usr/bin/id          id
```