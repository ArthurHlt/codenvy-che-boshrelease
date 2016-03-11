Eclipse-che BOSH release
=======================

[Che](https://eclipse.org/che/) Eclipse Che is a developer workspace server and cloud IDE running on BOSH in AWS, vSphere, GCE, Azure, OpenStack and more.

![sample1](https://eclipse.org/che/images/hero-home.png)

Requirements
------------

- [Docker Bosh release](https://github.com/cloudfoundry-community/docker-boshrelease): **required version V23**

Deployment
----------

Let's have a look on folder [manifest-examples](manifest-examples).

You will see two types of template:

- **1VM**: Let you run che and docker in one VM
- **Default**: Run che and docker in separate VM this will help you to scale the number of docker daemons (you could use [docker swarm](https://docs.docker.com/swarm/overview/) which is configurable in docker-boshrelease, see: https://github.com/cloudfoundry-community/docker-boshrelease/blob/master/SWARM.md )
