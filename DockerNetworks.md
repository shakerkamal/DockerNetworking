## Definition

Whenever a docker container is spun up, under the hood the container configures the networking. It caters to the network namespace, virtual ethernet, and route table. Without this information, the containers will not be able to talk to each other or even connect to the internet.

## Docker Networking
Docker has three types of networks:
1. Bridge
   - The containers has an ip same range as of the docker host. Suppose there are 3 containers, the ip addresses will be 172.15.0.2, 172.15.0.3, 172.15.0.5 and the ip of the docher host is 172.15.0.1. Command: `docker run ubuntu`
2. Host
   - The container has the same ip address and port as of the host. If a container runs on 192.29.0.14:5000 then no other container can run on port 5000, since it has already occupied the port 5000. Command: `docker run ubuntu --network=host`
3. None
   - The container is completely isolated from outside access of the docker host. Since it is not connected to any network. Command: `docker run ubuntu --network=none`

## Creating multiple newtworks in Docker host
Docker host by default has a single network. To create another network the following commands need to be executed:
```
docker network create \
  --driver bridge \
  --subnet 182.18.0.0/16
  custom-isolated-network
```

It will create a seperate network and containers can be assigned to this network.

## Prerequisites

- Linux image (Ubuntu preferred)
- net-tools
- basic understanding of Linux commands
- ifconfig
- ip addr
- telnet
- route
- arp

## Steps to follow

> install related network tools
> create namespaces
> create virtual ethernet (veth) cable
> connect the veth cable to the two network namespaces
> start/enable the ethernet devices of each network namespace
> assign ip addresses
> configure route tables if not configured
> ping to check if they are connected

# Install network tools

To create namespaces with this command

`sudo apt install net-tools`

# Create Namespaces

To create a network namespace we need net-tools command. Create a network namespace with the following command

```
sudo ip netns add red
sudo ip netns add green
```

Verify if the namespaces are created

`ip netns list`

The list should output like this

```
green (id: 1)
red (id: 0)
```

# Create a Virtual Ethernet cable

Create the virtual ethernet (veth) cable with

`sudo ip link add veth-red type veth peer name veth-green`

Verify the network interface list with `ip link list`

# Connect the networks with the veth cable

