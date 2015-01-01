docker-bitcoind
===============

Docker image that runs a bitcoind node in a container for easy deployment.

Credits
-------

This is just a fork of [kylemanna/docker-bitcoind](https://github.com/kylemanna/docker-bitcoind) with only minor tweaks

Requirements
------------

* Machine, Cloud, or VPS that supports Docker (i.e. EC2 Digital Ocean, KVM or XEN based VMs) running something like Ubuntu 14.04 or later (not OpenVZ containers!)
* At least 30 GB to store the block chain files
* 1GB RAM (maybe less, haven't test)

Tested on Digital Ocean's 1GB / 1 CPU / 30GB droplet.

Really Fast Quick Start
-----------------------

One liner for Ubuntu 14.04 LTS machines with JSON-RPC enabled on localhost:

    $ curl https://raw.githubusercontent.com/hectorj/docker-bitcoind/master/bootstrap-host.sh | sh -s trusty

Quick Start
-----------

1. Create a bitcoind-data volume to persist the bitcoind blockchain data, should exit immediately.  The bitcoind-data container will store the blockchain when the node container is remade later (software upgrade, reboot, etc):

        $ docker run --name=bitcoind-data hectorj/bitcoind init

2. Run a Bitcoin node and use the data volume:

        $ docker run --volumes-from=bitcoind-data --name=bitcoind-node -d -p 8333:8333 hectorj/bitcoind
        5144bdf31fa689e166fe3a8e1a3befd2b28bbb1bd48207f4583c072207124a10

3. Verify that the container is running:

        $ docker ps
        CONTAINER ID        IMAGE                       COMMAND                CREATED             STATUS              PORTS                              NAMES
        5144bdf31fa6        hectorj/bitcoind:latest   /bitcoin/bitcoind.sh   6 seconds ago       Up 5 seconds        0.0.0.0:8333->8333/tcp, 8332/tcp   bitcoind-node

4. You can then access the daemon's output thanks to the [docker logs command]( https://docs.docker.com/reference/commandline/cli/#logs) (with the container id given by `docker ps`)

        $ docker logs -f 5144bdf31fa6

Debugging
---------

    $ docker run --volumes-from=bitcoind-data --rm -it -p 8333:8333 hectorj/bitcoind shell


Enable JSON-RPC
---------------

The following Docker run line will create a container with JSON-RPC enabled and will only allow Docker host to access the JSON RPC port 8332.

    $ docker run --volumes-from=bitcoind-data --name=bitcoind-node -d -p 8333:8333 -p 127.0.0.1:8332:8332 hectorj/bitcoind bitcoind -disablewallet -rpcallowip=*


Todo
----

- [ ] Add Ubuntu 14.04 quick start guide
- [ ] Add Ubuntu upstart init script
- [ ] Review possiblity of bootstraping blockchain via BitTorrent
