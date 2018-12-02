# Docker Deep Dive

- [Architecture Big Picture](#architecture-big-picture)
- [Kernel Internals](#kernel-internals)
    * [Namespaces](#namespaces)
        + [Hypervisors](#hypervisors)
        + [Containers](#containers)
    * [Control Groups](#control-groups)
- [Docker Engine](#docker-engine)


## Architecture Big Picture

<p align="center">
    <img src="https://user-images.githubusercontent.com/29547780/40360546-748d9f4e-5dbe-11e8-8a8b-567e6c6ef254.png">
</p>

Firstly a container by definition is an _isolated area of an OS with resource usage limits applied_.

Now, to build them, we've leverage a bunch of Kernel low level stuff. In particular we use _Namespaces_ and _Control Groups_ as below.

<p align="center">
    <img src="https://user-images.githubusercontent.com/29547780/40361125-350c9dfa-5dc0-11e8-946d-9b615bee7e61.png">
</p>

The _Docker Engine_ is just like any other engine, it is modular.
We might interface with it through the CLI but under the covers it's just a bunch of smaller moving parts.

<p align="center">
    <img src="https://user-images.githubusercontent.com/29547780/40476138-31ca726c-5f3b-11e8-8fef-c5596a56a1ec.png">
</p>

So the workflow of the diagram above is as follow:

* We use a command line to create a container: `docker container run`
* The client takes the command and makes the appropriate API request to the _containers/create_ endpoint in the engine.
* The docker engine then pulls all of the required kernel stuff _(namespaces, groups)_ and _pop!_ Out comes a container.

## Kernel Internals

* The stuff( _namespsaces, control groups_ ) to build _Containers_ is *NOT* new.
* There are two main building blocks when creating containers. _Namespaces_ and _Control Groups_.
* Both of them are Linux Kernel primitives.
* _Namespaces_ are about isolation and _Control Groups_ are about grouping objects and setting limits.

### Namespaces

<p align="center">
    <img src="https://user-images.githubusercontent.com/29547780/40477855-6fa00aa8-5f3f-11e8-87a1-89d5bd795388.png">
</p>

* These let us take an operating system and carve into multiple isolated virtual operating systems. It's a bit like _Hypervisors_ in _Virtual Machines_.

#### Hypervisors

<p align="center">
    <img src="https://user-images.githubusercontent.com/29547780/40479600-f6600dbe-5f43-11e8-80a1-a017ddb1e2a3.png">
</p>

* In the _hypervisors_ world we take a single physical machine with all it's resources ( _CPU, RAM_ )
and we carve out one or more virtual machines.
* Each virtual machine gets its own virtual CPU, virtual RAM, virtual memory, virtual networking.... the whole shabang ( _the whole lot_ )!

#### Containers

<p align="center">
    <img src="https://user-images.githubusercontent.com/29547780/40488508-d7a55436-5f5e-11e8-8907-b648b55ebc34.png">
</p>

* Use of namespaces to take a single operating system with all of its resources ( _file systems, process trees, users..._ )
and carve all that up into multiple virtual operating systems which are called `Containers` .
* Each container gets its own virtual/containerized _root file system_, it's own _process tree_ and etc...
* Just as the _hupervisor_ looks and tastes just like a regular and physical server, in the container world, each container looks
and smells exactly like the operating system except, _it's not_. All Containers share a single _kernel_ on a host.
* Everything is isolated so stuff in _container A_ won't even know about other containers _B_ and _C_ sharing the same _kernel host_.

---

<p align="center">
    <img src="https://user-images.githubusercontent.com/29547780/40539469-bfed5b0a-600c-11e8-9436-33ef0200ea32.png">
</p>

* In the Linux world there are these :point_up: namespaces. 
* A docker container is basically an organised collection of namespaces.
* So the container above is its own isolated grouping of these namespaces, so its got its own _pid, net, ipc, uts etc.._.
* Each container is secure and has a secure boundary.


    - Process ID (pid) - gives each container its own isolated process tree, complete with its very own _pid 1_. 
    This means that one container is totally unaware of the existence of any other containers.
    - Network (net) - gives each container it's own isolated network stack. So it's a mix e.g. _Ip Tables_ and etc...
    - Filesystem/mount(mnt) - gives each container it's own isolated root file system e.g. `C:/` on Windows and `/` on Linux.
    - Inter-proc comms(ipc) - lets processors in a single container access the same shared memory, but it stops everything outside of the container. 
    - UTS - gives every container its own host name.
    - User - lets you map accounts inside a container to different users on a host.
    
### Control Groups

<p align="center">
    <img src="https://user-images.githubusercontent.com/29547780/49346120-8c50f800-f685-11e8-8b79-4cecf2c829ee.png">
</p>

* Like any multi-tenant system, there is always the fear of _noisy neighbours_.
* The last thing `Container A` wants is `Container D` to start chewing all the _CPU_ and _RAM_.
* So in this case we need something to _police_ the consumption of system resources. In the _linux_ world,
this is called `Control Groups/ C-Groups`. In the windows_ world, this is called `Job Objects`.
* The idea here of Control Groups is to group processes and impose limits.

## Docker Engine