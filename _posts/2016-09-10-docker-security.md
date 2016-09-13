---
layout: post
title: Docker Security
---


### 概述
![Docker Security modules](/public/images/docker security modules.png)

 * Namespaces – PID, Mount, Network, IPC, UTC, User.

 * Cgroups – Limit CPU, Memory, IO

 * Capabilities – Reduced root access. 36 capabilities to control as of kernel 3.19.0.21.

 * Seccomp profiles – Control kernel system calls. 300+ system calls available that can be controlled using these profiles.

 * Special kernel modules like AppArmor, SELinux – Provides granular control over Kernel resources.

 * Docker client access Docker engine
   ![docker client coummunication with engine](/public/images/docker client communication engine.png)

### 威胁

* Kernel exploits: 所有进程运行在同一个内核中，即使有多个容器，所有的系统调用其实都是通过主机的内核处理，因此该内核中存在的任何安全漏洞都有可能造成巨大影响。如果某套容器系统导致内核崩溃，那么这反过来又会造成整台主机上的全部容器毁于一旦

* Denial-of-service attacks: 所有容器都共享同样的内核资源。如果某套容器能够以独占方式访问某些资源，那么与其处于同一台主机上的其它容器则很可能因资源匮乏而无法正常运转

* Container breakouts: 通过获取某个容器的访问而能访问其他的容器或是主机

* Poisoned images：镜像来源安全可靠

* Docker client access engine,docker-machine,Docker registry:

* Network的设置

* Compromising secrets: 访问服务或是数据库等的安全(容器间服务访问)

* Application Security: 容器内应用程序本身的安全问题

### 解决

* Defense in Depth：实施多层次的防御策略

* Use TLS and use self-define network

* Least Privilege：容器内不要root用户运行程序、设置文件，容器，数据卷仅只读权限、减少内核调研、限制资源(如：CPU、内存等)

* Applying Updates：方便快捷的更新补丁的能力

* Segregate Containers by Host

* Image Provenance

##### Security Tips

![](/public/images/docker security toolbox.png)

1. Set a User: RUN groupadd -r user_grp && useradd -r -g user_grp user USER user

2. Limit Container Networking

3. Remove setuid/setgid Binaries “find / -perm +6000 -type f -exec ls -ld {} \”

4. Limit Memory,CPU,Restarts, Filesystems,Capabilities

5. Enforce SELinux Policies

6. Disiable inter-container communication on bridge0


### 工具

* **Docker Bench for Security** The Docker Bench for Security is a script that checks for dozens of common best-practices around deploying Docker containers in production: https://github.com/docker/docker-bench-security
![](https://raw.githubusercontent.com/docker/docker-bench-security/master/benchmark_log.png)

* **Clair** Clair is an open source project for the static analysis of vulnerabilities in appc and docker containers.
https://github.com/coreos/clair

* **Lynis** Lynis is an open source security auditing tool. Used by system administrators, security professionals, and auditors, to evaluate the security defenses of their Linux and UNIX-based systems
https://cisofy.com/lynis/
