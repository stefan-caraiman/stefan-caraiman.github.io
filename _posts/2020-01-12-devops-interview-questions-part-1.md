---
title: "DevOps interview questions - Part 1"
date: 2020-01-12T10:23:30-04:00
categories:
  - devops
  - interview-questions
  - linux
tags:
  - docker
  - linux
  - devops
  - interview
  - debugging
  - chmod
---

## Context

In the past few years I've been in quite a number of interviews, both as an interviewer and interviewee. 

And while I agree to some degree that anyone which calls themselves a DevOps/Cloud/Platform Engineer should have a base knowledge about networking, OS, scripting, etc. at the end of the day that's not what differences an average candidate which knows things by the book from an ideal one that can tackle anything issue thrown at them.

Thus I've come to the conclusion that in order to make that difference, the "strangest" and "quirkest" questions are the best for figuring out how well and outside the box a candidate can think when thrown against an unpredictable question. 

Because, let's be real here, our day to day job is not something which you can find in "step by step" guide. It's a mix of previous experience, knowing where to look for information, and how to apply and combine all of these in your particular issue at hand.

Having said that, I will be going through some "non-standard" questions(and the solutions) which I've came across along the years.

## The chmod 444 issue

Let's take the following scenario. A careless administrator(maybe even yourself) has run the following command:

```console
$ # Set permissions to read-only for the chmod binary
$ chmod 444 /bin/chmod
$ # Try and set executable permissions back
$ chmod +x /bin/chmod
bash: /bin/chmod: Permission denied
```

Oops, looks like we're not able to run `chmod` anymore, what ways can you think of to fix this? Bonus points, don't use `ssh`, `scp` to copy the binary from another system.

### Solutions

1. Copy permissions from another binary and overwrite the binary file, thus keeping the permissions

```console
$ cp /bin/ls chmod.01
$ cp /bin/chmod chmod.01
$ ./chmod.01 +x /bin/chmod
```

This works since `cp` preserves permissions by default.

2. Use the `perl`, `python` or `ruby` modules for setting the permissions

```console
$ perl -e 'chmod(0755, "chmod")'
$ ruby -r fileutils -e "FileUtils.chmod 0755, '/bin/chmod'"
$ python -c "import os; os.chmod('/bin/chmod', 0755)"
```

3. Using `busybox`

Since busybox has all of the core Unix utilies built-in, we can just use the `chmod` from there.

```console
$ busybox chmod 755 /bin/chmod 
```

4. Using `rsync` with the `chmod` argument to set the permissions of the file

This one is pretty similar to the `cp` one, still an elegant solution.

```console
$ rsync /bin/chmod /tmp/chmod --chmod=ugo+x
$ /tmp/chmod +x /bin/chmod
```

## Restore a deleted, but still running script

Another careless administrator has accidentally deleted an important script from the filesystem. The file isn't versioned or backed up anywhere, but the process which started the script is still running.

How would you restore it?

### Example:

```console
$ cat <<EOF > script.sh
#!/usr/bin/env bash
while true; do
sleep 1;
done
EOF
$ bash script.sh &
[1] 1997
$ rm -vf script.sh
removed 'script.sh'
$ # Let's check the open file descriptor
$ lsof -p 1997 | grep script.sh
bash    1997 test  255r   REG    8,1       49  3579421 /home/test/script.sh (deleted)
$ # As we can see, the file is deleted, but fret not, we can just cat the file descriptor which contained the file, like so:
$ cat /proc/1997/fd/255 
#!/usr/bin/env bash
while true; do
sleep 1;
done
$ cp /proc/1997/fd/255 script.sh
$ # Hooray! We got the file content back
```

### Explanation

But how does this work?

Every process on the system has a directory in `/proc/` with it's ID in it, inside of which lies many things regarding the process, including an `fd` (file descriptor) subdirectory containing links to all files that the process has open. Even if a file has been removed from the filesystem, a copy of the data will still be there.

The only issue with this, is that once the process stops the file descriptors are gone. So in the end, you should always make sure that you version control or backup your important scripts somehow.

## Conclusion

The problems above can also serve as a segue into more detailed topics, such as `What is a file descriptor?`, `What else can you find in the /proc/ folder?`, `What are permissions?` and so on, making the interview process even more fluid and less nerve breaking.

This will be a multi-part series of posts containing various DevOps/Linux interview questions, so make sure to come back for more if this one caught your interest.

Looking forward to what you think about the questions above and other possible solutions to them, happy learning!
