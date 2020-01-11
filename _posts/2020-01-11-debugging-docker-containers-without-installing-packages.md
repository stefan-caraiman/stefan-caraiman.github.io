---
title: "Debugging docker containers without installing packages"
date: 2020-01-11T20:30:30-04:00
categories:
  - docker
  - linux
tags:
  - docker
  - linux
  - nsenter
  - debugging
---

Imagine the following scenario, you have a running container which is mis-behaving in production and you have to debug it, but the container does not have the necessary tooling. And for various reasons(and also as a best practice) you can not install any 3rd party packages inside the running container in order to debug the issue at hand.

Fret not! You don't even need to install those debug tools inside the container, you can just use the binaries which are already residing on the host containing the container.

## What's the solution?

The answer is `nsenter` !

`nsenter` allows you to enter a specific process namespace, for example in this scenario, it allows you to enter the network namespace of the container and still keep access to the host tooling. You can read more about it [here][nsenter].

Keep in mind that that any kind of namespace change requires you to have root privileges. Now let's see this in action:

```console
$ docker run -itd --rm --name example nginx:latest
$ docker inspect example --format '{{ .NetworkSettings.SandboxKey }}'
/var/run/docker/netns/090e6afd5551
$ nsenter --net=/var/run/docker/netns/090e6afd5551
$ # Let's try listing all current interfaces with ip
$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
8: eth0@if9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
$ # What about opened ports?
$ netstat -nlp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      15098/nginx: master
```

## Conclusion

Et voilà! Any command from this point on will be ran inside the container's namespace. Don't forget to `exit` the namespace once you are done debugging.

I look forward to your comments and any questions you might have, happy debugging!

[nsenter]: http://man7.org/linux/man-pages/man1/nsenter.1.html
