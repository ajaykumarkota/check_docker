[![Build Status](https://travis-ci.org/timdaman/check_docker.svg?branch=master)](https://travis-ci.org/timdaman/check_docker)
[![Code Climate](https://codeclimate.com/github/timdaman/check_docker/badges/gpa.svg)](https://codeclimate.com/github/timdaman/check_docker)
[![Test Coverage](https://codeclimate.com/github/timdaman/check_docker/badges/coverage.svg)](https://codeclimate.com/github/timdaman/check_docker/coverage)
# check_docker
This a a nagios/NRPE compatible plugin for checking docker containers. So far you can use it to check

- memory consumption in absolute units (bytes, kb, mb, gb) and as a percentage (0-100%)
  of the container limit.
- CPU usages as a percentage (0-100%) of container limit.
- automatic restarts performed by the docker daemon
- container status, i.e. is it running?
- container health checks are passing?
- uptime, i.e. is it able to stay running for a long enough time?
- image version (experimental!), does the running image match that in
  the remote registry?

This check can communicate with a local docker daemon socket file (default) or with local
or remote docker daemons using secure and non-secure TCP connections.

This plugin requires python 3. It is tested on 3.3 and greater but may work on older
versions of 3.

## Installation

With pip

    pip3 install check_docker
    --or--
    pip install check_docker

With curl

    curl -o /usr/local/bin/check_docker https://raw.githubusercontent.com/timdaman/check_docker/master/check_docker
    chmod a+rx /usr/local/bin/check_docker

With wget

    wget -O /usr/local/bin/check_docker https://raw.githubusercontent.com/timdaman/check_docker/master/check_docker
    chmod a+rx /usr/local/bin/check_docker


## Usage

    usage: check_docker.py [-h]
                           [--connection [/<path to>/docker.socket|<ip/host address>:<port>]
                           | --secure-connection [<ip/host address>:<port>]]
                           [--timeout TIMEOUT]
                           [--containers CONTAINERS [CONTAINERS ...]]
                           [--cpu WARN:CRIT] [--memory WARN:CRIT:UNITS]
                           [--status STATUS] [--health] [--uptime WARN:CRIT]
                           [--version] [--restarts WARN:CRIT]
  
    Check docker containers.
  
    optional arguments:
      -h, --help            show this help message and exit
      --connection [/<path to>/docker.socket|<ip/host address>:<port>]
                            Where to find docker daemon socket. (default:
                            /var/run/docker.sock)
      --secure-connection [<ip/host address>:<port>]
                            Where to find TLS protected docker daemon socket.
      --timeout TIMEOUT     Connection timeout in seconds. (default: 10.0)
      --containers CONTAINERS [CONTAINERS ...]
                            One or more RegEx that match the names of the
                            container(s) to check. If omitted all containers are
                            checked. (default: ['all'])
      --cpu WARN:CRIT       Check cpu usage percentage taking into account any
                            limits. Valid values are 0 - 100.
      --memory WARN:CRIT:UNITS
                            Check memory usage taking into account any limits.
                            Valid values for units are %,b,k,m,g.
      --status STATUS       Desired container status (running, exited, etc).
                            (default: None)
      --health              Check container's health check status
      --uptime WARN:CRIT    Minimum container uptime in seconds. Use when
                            infrequent crashes are tolerated.
      --version             Check if the running images are the same version as
                            those in the registry. Useful for finding stale
                            images. Only works with public registry.
      --restarts WARN:CRIT  Container restart thresholds.

Gotchas: 

* When using this with older versions of docker (I have seen 1.4 and
1.5) –status only supports ‘running’, ‘restarting’, and ‘paused’.
* When no container is specified all containers are checked. Some containers will return critcal status because the 
selected check(s) require a running container. 
