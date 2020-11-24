# Docker Networking

## The Basics
- Networks are huge, complex and central.

### Background
- Docker didn't focus on networking in the beginning. 
- As Docker became more and more popular, the networking aspect was a pain with the use of `--links`, etc.
- Docker acquired [socketplane](https://socketplane.io/) which specialised in the field of networks and eventually
improved Docker's networking system.


### The Three Pillars of Docker Networking

- Container Network Model (Most important)
    - Grand design behind everything else.
    - DNA of Docker Networking.
    - Libnetwork + Drivers draw from the same DNA.
    - This is a specification document hosted on [Github](https://github.com/moby/libnetwork/blob/master/docs/design.md).
    - This is a spec proposed by Docker inc. So everything about it is aimed at perfecting networking for Docker.
    - CNM vs CNI
        - Container Network Interface.
        - The CNI specification is more suited for Kubernetes environments.
    - Defines 3 major constructs:
        - Sandbox
            - Just means a `namespace`.
            - Isolated area of the OS(Operating System).
            - Contains full network stacks e.g. `ipconfig, DNS routing tables...`.
        - Endpoint
            - Network interfaces e.g. `eth0`.
        - Network
            - Connected endpoints that can talk to each other.
            
![Container Network Model](https://user-images.githubusercontent.com/29547780/100031562-d93bb500-2ded-11eb-9925-e70b6a3e79fb.png)

- Libnetwork
    - Real world implementation of the CNM by Docker, Inc.
    - The CNM is the design and [Libnetwork](https://github.com/moby/libnetwork) is the implementation of that design in GoLang code.
    - Central place for all Docker networking logic, API, UX etc...
    - X-platform as it had to run almost on any platform, Windows, Linux etc...
    
- Drivers
    - Sits on top of the Libnetwork.
    - This is where the Network-specific details live.
    - Each Network type has its own driver:
        - Overlay driver = Overlay Networks.
        - MACVLAN driver = MACVLAN Networks.
        - IPVLAN driver = IPVLAN Networks.
        - Bridge driver = Bridge Networks.

So in a nutshell: 
`CNM (Design/DNA) -----> LibNetwork (Control plane & management plane) -----> Drivers (Data plane)`
