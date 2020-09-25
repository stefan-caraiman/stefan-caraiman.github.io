---
title: "TryHackMe Wonderland Writeup 👨‍💻"
date: 2020-09-25T12:22:31-04:00
categories:
  - linux
  - tryhackme 
tags:
  - linux
  - tryhackme
  - hack
  - tutorial
  - walkthrough
---

This post serves as a walkthrough for the **Wonderland** room challenge on the **TryHackMe** platform.

I've had quite a lot of fun with this one and learned quite a lot of new tools, so for anyone interested into **hacking** and the process behind it, feel free to try the [room][room] as well by yourself. 🍀

**Note ❗:** Beware that this blogpost is trying to be as **beginner level** friendly as possible, as I will try to also explain some of the tools and logic that goes into trying to break into a machine. 


## Information Gathering ✍

The first step with any kind of CTF(*Capture The Flag*) challenge is to start gathering any kind of information about the machine. Clasically this means to start off by doing a **port scanning** of the host IP.

This is usually done by using the **nmap** tool, an open source tool for network exploration and security auditing. Basically we are checking what **open ports** there are on the system, like so:


```console
$ export IP=10.10.21.22
$ mkdir nmap
$ nmap -p- -T4 -sC -sV -vvv -oA nmap/scan $IP

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 8e:ee:fb:96:ce:ad:70:dd:05:a9:3b:0d:b0:71:b8:63 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDe20sKMgKSMTnyRTmZhXPxn+xLggGUemXZLJDkaGAkZSMgwM3taNTc8OaEku7BvbOkqoIya4ZI8vLuNdMnESFfB22kMWfkoB0zKCSWzaiOjvdMBw559UkLCZ3bgwDY2RudNYq5YEwtqQMFgeRCC1/rO4h4Hl0YjLJufYOoIbK0EPaClcDPYjp+E1xpbn3kqKMhyWDvfZ2ltU1Et2MkhmtJ6TH2HA+eFdyMEQ5SqX6aASSXM7OoUHwJJmptyr2aNeUXiytv7uwWHkIqk3vVrZBXsyjW4ebxC3v0/Oqd73UWd5epuNbYbBNls06YZDVI8wyZ0eYGKwjtogg5+h82rnWN
|   256 7a:92:79:44:16:4f:20:43:50:a9:a8:47:e2:c2:be:84 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBHH2gIouNdIhId0iND9UFQByJZcff2CXQ5Esgx1L96L50cYaArAW3A3YP3VDg4tePrpavcPJC2IDonroSEeGj6M=
|   256 00:0b:80:44:e6:3d:4b:69:47:92:2c:55:14:7e:2a:c9 (EdDSA)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAsWAdr9g04J7Q8aeiWYg03WjPqGVS6aNf/LF+/hMyKh
80/tcp open  http    syn-ack Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Follow the white rabbit.
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

As we can see, we only have two ports open, **22 for SSH** and **80 for HTTP**, lets try to access the HTTP webpage(http://10.10.21.22 in my case):






[room]: https://tryhackme.com/room/wonderland
