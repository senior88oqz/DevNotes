# Docker container networking

A container attached to a Docker network will get a unique IP address that is routable from other containers attached to the same Docker network.

Networks are treats  as first-class entities in Docker. They have their own life cycle and are *not* bound to any other objects.

By default Docker includes ***three*** networks where each is provided by a different driver:

```bash
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
04164f93085c        bridge              bridge              local
e5790991a29a        host                host                local
482cfd244aed        none                null                local
```

* The `bridge` driver provides inter-container connectivity for all containers running on the same machine.
* The `host` driver instructs Docker not to create any special networking namespace or resources for attached containers.
* Containers attached to the `none` network with `null` driver will not have any network connectivity outside of themselves.
* The `scope` of a network can take three values, `local`, `global`, or `swarm`.
* All of the default networks have the `local` scope, and will not be able to route traffic between containers running on different machines directly.

![Default local Docker network topology with two attached containers](images/default_network.png)

**NOTE** : Using the default bridge network is NOT recommended. It cannot take advantage of modern Docker features like *service discovery* or *IPVS based load balancing*.

## Bridge Network

