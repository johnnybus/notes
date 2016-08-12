# Docker Architecture
Docker uses a client-server architecture. The docker client talks to the docker deamon, which will does de heavy lifting of bulding, running and distributing your docker containers. The Docker client and daemon communicate via sockets or through a RESTful API.

![Docker Architecture - Comunication](imgs/docker-arch1.svg)

## Under the hood

#### Main technologies:
- Control Groups (cgroups)
- Namespaces
- The Libcontainer Specification

### Namespaces
A [namespace](http://man7.org/linux/man-pages/man7/namespaces.7.html) wraps a global system resource in an abstraction that makes it appear to the processes within the namespace that they have their own isolated instance of the global resource.  Changes to the global resource are visible to other processes that are members of the namespace, but are invisible to other processes.  One use of namespaces is to implement containers.

When a container is run, Docker creates a set of namespaces for that container. Each aspect of the container runs inside a namespace.

##### Some of the namespaces that Docker Engine uses on Linux are:
- The pid namespace: Process isolation (PID: Process ID).
- The net namespace: Managing network interfaces (NET: Networking).
- The ipc namespace: Managing access to IPC resources (IPC: InterProcess Communication).
- The mnt namespace: Managing mount-points (MNT: Mount).
- The uts namespace: Isolating kernel and version identifiers. (UTS: Unix Timesharing System).

### Control Groups (cgroups)
[Control Groups](https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt) provide a mechanism for aggregating/partitioning sets of
tasks, and all their future children, into hierarchical groups with
specialized behaviour.
This ensures containers are good multi-tenant citizens on a host. Control groups allow Docker Engine to share available hardware resources to containers and, if required, set up limits and constraints. For example, limiting the memory available to a specific container.

### Union File Systems
Union file systems, or UnionFS, are file systems that operate by creating layers, making them very lightweight and fast. Docker Engine uses union file systems to provide the building blocks for containers. Docker Engine can make use of several union file system variants including: AUFS, btrfs, vfs, and DeviceMapper.

###### AUFS
UnionFS works at a directory level, as opposed to a device level, so it dosen't have a mount point - it sits over existing mount points each of which might be a branch - for example, having a base layer (or to use proper terminology - a low precedence layer with the root filesystem) on a read only iso9660 cd rom file system, and a branch on a ramdisk. Each branch is assigned a precedence and a branch with a higher precedence overrides one of a lower precedence.

If a directory exists in two underlying branches, the contents and attributes of the Unionfs directory are the combination of the two lower directories.

If a file exists in two branches, the contents and attributes of the Unionfs file are the same as the file in the higher-priority branch, and the file in the lower-priority branch is ignored.

Finally if there's a duplicate, the duplicate directory is hidden to simplify things.

[Linuxjournal Article](http://www.linuxjournal.com/article/7714)

### Container format
Docker Engine combines these components into a wrapper we call a container format. The default container format is called [libcontainer](https://github.com/docker/libcontainer). In the future, Docker may support other container formats, for example, by integrating with BSD Jails or Solaris Zones.

--
###### Sources:
- http://superuser.com/questions/326190/how-does-unionfs-work
- https://docs.docker.com/engine/understanding-docker/
- https://www.kernel.org/doc/Documentation/cgroup-v1/cgroups.txt
- http://man7.org/linux/man-pages/man7/namespaces.7.html
