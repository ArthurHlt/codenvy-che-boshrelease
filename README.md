Eclipse-che BOSH release
=======================

[Eclipse Che](https://eclipse.org/che/) is a developer workspace server and cloud IDE running on BOSH in AWS, vSphere, GCE, Azure, OpenStack and more.

![sample1](https://eclipse.org/che/images/hero-home.png)

**Note**: Che packaged here is a release candidate.

Requirements
------------

- [Docker Bosh release](https://github.com/cloudfoundry-community/docker-boshrelease): **required version V23**

Deployment
----------

Let's have a look on folder [manifest-examples](manifest-examples).

You will see two types of template:

- **1VM**: Let you run che and docker in one VM
- **Default**: Run che and docker in separate VM this will help you to scale the number of docker daemons (you could use [docker swarm](https://docs.docker.com/swarm/overview/) which is configurable in docker-boshrelease, see: https://github.com/cloudfoundry-community/docker-boshrelease/blob/master/SWARM.md )

In this folder, only examples are present update them for your own IaaS and deploy it with bosh.

## Usage

To use this bosh release, first upload it to your bosh:

```
bosh target BOSH_HOST
git clone https://github.com/cloudfoundry-community/eclipse-che-boshrelease.git
cd eclipse-che-boshrelease
bosh upload release releases/eclipse-che/eclipse-che-5.yml
```

For [bosh-lite](https://github.com/cloudfoundry/bosh-lite), you can quickly create a deployment manifest & deploy a cluster:

```
templates/make_manifest warden
bosh -n deploy
```

For AWS EC2, create a single VM:

```
templates/make_manifest aws-ec2
bosh -n deploy
```

### Override security groups

For AWS & Openstack, the default deployment assumes there is a `default` security group. If you wish to use a different security group(s) then you can pass in additional configuration when running `make_manifest` above.

Create a file `my-networking.yml`:

```yaml
networks:
  - name: eclipse-che1
    type: dynamic
    cloud_properties:
      security_groups:
        - eclipse-che
```

Where `- eclipse-che` means you wish to use an existing security group called `eclipse-che`.

You now suffix this file path to the `make_manifest` command:

```
templates/make_manifest openstack-nova my-networking.yml
bosh -n deploy
```

### Development

As a developer of this release, create new releases and upload them:

```
bosh create release --force && bosh -n upload release
```


Available properties
--------------------

### Job che

- **che.docker.host.ip**: The IP address of the Docker host.
- **che.docker.host.port**: The port number of the Docker host.
- **che.external_ip**: Specify on what ip che is available. By default, if it's not specified, release will choose the first ip of the network.
- **che.docker.registry.host**: (Optional) The IP address of the Docker registry host.
- **che.docker.pull_image**: (Optional) If this is true, then we always pull an image from a registry even if we have an image cached locally. If false, Docker only pulls image if it does not exist locally.
- **che.secure**: (Optional) If this is true, it will force to use https. If false, it will use http.
- **che.github.clientId**: (Optional) The client id to bind che to github. If set che.github.clientSecret is needed
- **che.github.clientSecret**: (Optional) The client secret to bind che to github. If set che.github.clientId is needed
- **che.env.http_proxy**: HTTP proxy that che should use
- **che.env.https_proxy**: HTTPS proxy that che should use

### Job che-inject

- **che.ip**: Specify on what ip che is available. This is needed or che couldn't reach docker