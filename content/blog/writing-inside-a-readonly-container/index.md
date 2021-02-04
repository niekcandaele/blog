---
title: Writing inside a read-only container
date: "2021-02-04"
categories: [containers]
comments: true
---

So docker run has a cool option that not many people know about: `--read-only` will mount the container's root filesystem as read only. Depending on your environment, this can be a very interesting security feature. However, a lot of applications don't run well in a full read-only filesystem. Often times applications have to write some temp files, cache something on disk, ...

Here's a method I used to get around that. This keeps the container file system as read-only but allows you to open up certain directories for writing.

```Dockerfile
FROM alpine:3

RUN mkdir /test_folder_ro
RUN mkdir /test_folder_rw

VOLUME [ "/test_folder_rw" ]

ENTRYPOINT [ "sh" ]
```

Let's see what's happening here.

- `FROM alpine:3` This is the base image, I used Alpine here because it's small.
- `RUN mkdir /test_folder_r*` With these commands, I create 2 folders. One will be read-only (ro) while the other will be read-write (rw)
- `VOLUME [ "/test_folder_rw" ]` This is the magic sauce 😁
- `ENTRYPOINT [ "sh" ]` This tells the container to open a shell when it starts

So let's build this image and run it!

```sh
docker build -t test:latest
docker run --rm -it --read-only test:latest
```

First things first, let's make sure our rw folder is writeable and the ro folder isn't.

```
/ # touch /test_folder_rw/test
/ # touch /test_folder_ro/test
touch: /test_folder_ro/test: Read-only file system
```

Awesome!

But why does declaring a volume make the folder writeable? We're not even using a volume in our `docker run` command!

To understand this, we should take a look at what the filesystem looks like inside the container. We're actually mostly interested in what's mounted as read-only so we can use `grep 'ro' /proc/mounts` to get a list. It's a long list, so I'm not going to copy it here but the important bit is that the root `/` is mounted as readonly. That's to be expected ofcourse, but what about that rw folder?

```
/ # grep 'test_folder_r' /proc/mounts
/dev/sdb5 /test_folder_rw ext4 rw,relatime,errors=remount-ro 0 0
```

Aha! So by declaring a volume, we actually created a new mountpoint which is writable. Only the folder where we declared a volume is writeable, the rest of the file system is still read-only.
