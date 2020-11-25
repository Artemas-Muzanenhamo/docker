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

### Hands on Basic Docker Networking Commands

- List container networks:

```shell script
$ docker network ls

NETWORK ID          NAME                        DRIVER              SCOPE
c6e6b66865bc        bridge                      bridge              local
cb81fa2b19f2        flux-book-store_default     bridge              local
e074bb3fd965        host                        host                local
552bb7578b88        none                        null                local
a10eea6b9cb9        party-reservation_default   bridge              local
```
- Every network get a unique ID, a name, which driver owns the container and the scope.

- Get more information on a specific container network:

```shell script
$ docker network inspect party-reservation_default

[
    {
        "Name": "party-reservation_default",
        "Id": "a10eea6b9cb91a31fa105e3f60197e8f37393205fac4cf5349be5c4604879d9e",
        "Created": "2020-09-09T01:13:17.8669175Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": true,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {
            "com.docker.compose.network": "default",
            "com.docker.compose.project": "party-reservation",
            "com.docker.compose.version": "1.26.2"
        }
    }
]
```

## Use Cases and Drivers

### Single-host Networking (With the bridge driver)

- Bridge networks are single host networks.

![single-host-networking](https://user-images.githubusercontent.com/29547780/100110510-ee075f80-2e64-11eb-84d6-29aab9381ac4.png)

- Create a network bridge
```shell script
$ docker network create -d bridge --subnet 10.0.0.1/24 ps-bridge
e7c69494a93b5a1b6f8455bf1ca43312e64dab7ca62c5252e9819e14c298169e
```
- Check for the created network
```shell script
$ docker network ls
NETWORK ID          NAME                        DRIVER              SCOPE
c6e6b66865bc        bridge                      bridge              local
cb81fa2b19f2        flux-book-store_default     bridge              local
e074bb3fd965        host                        host                local
552bb7578b88        none                        null                local
a10eea6b9cb9        party-reservation_default   bridge              local
e7c69494a93b        ps-bridge                   bridge              local
```
- Inspect the created network
```shell script
$ docker network inspect ps-bridge
[
    {
        "Name": "ps-bridge",
        "Id": "e7c69494a93b5a1b6f8455bf1ca43312e64dab7ca62c5252e9819e14c298169e",
        "Created": "2020-11-24T14:57:44.2541719Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "10.0.0.1/24"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```
