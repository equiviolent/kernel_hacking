# kernel_hacking
A gentle guide to walk through a brief introduction on how to get started with kernel hacking (Mostly personal studies and notes).

# Table of Contents
1. [Prerequisites](#prerequisites)
2. [Intro to kernel hacking](#intro-to-kernel-hacking)
   - [Who is kernel hacker](#who-is-a-kernel-hacker)
3. [Create an environment to kernel hacking](#Create-an-environment-to-kernel-hacking)

## Prerequisites
- Basic knowledge about C programming language.
- Just a little bit knowledge about x86 Assembly (Especially the intel syntax).
- Basic knowledge about Linux and operation system in general.

## Intro to kernel hacking
### Who is a kernel hacker
A kernel hacker is someone who contributes to the Linux kernel.

### Create an environment to kernel hacking
I started to want to understand better my OS and experience with it, as you may know it's not ideal modifying the main system as it may occasionally crush.
I suddenly discovered a way to virtualize my actual OS, yes hou heard it right I can pretty virtualize my OS with a little help of systemd, actually pretty huge help.
The advantage of this method is, I can pretty modify my system without crushing it, everything outside the /home directory would be ereased after a restart, that being said, I can modify my kernel without fear of crushing my host OS.

Let's dig deep to how I virtualize my OS:
```sh
systemd-nspawn \
        --directory=/ \
        --volatile=yes \
        -U \
        --set-credential=passwd.hashed-password.root:$(mkpasswd mysecret) \
        --set-credential=firstboot.locale:C.UTF-8 \
        --bind-user=myuser \
        -b
```

This would make an instance of my actual system but instead it's a virtualized system, means we can freely change without any fear.

The meaning of the flags:
   - `--directory=/` tells systemd-nspawn to run off the host OS' file hierarchy.
   - `--volatile=yes` enables volatile mode.
   - `-U` means we'll enable user namespacing, in fully automatic mode.
   - `-set-credential=passwd.hashed-password.root:$(mkpasswd mysecret)` passes a credential to the container.
   - `systemd-firstboot` service in the container to initialize /etc/locale.conf with this locale.
   - `--bind-user=myuser` binds the host user into the container.
