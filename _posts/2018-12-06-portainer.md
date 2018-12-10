---
layout: post
title: Portainer.io
subtitle: Docker management tool.
comments: true
tags: Portainer, Docker
---

If you struggle to get your docker containers, images, repositories and others into control, there is a perfect tool for you: `Portainer.io`. This is open source, docker management tool which works on Linux, Windows and Mac.

![Portainer](/images/portainer.png)

# Installation

You can deploy it very simple using following docker-compose.yaml:

```yaml
version: '2'

services:
  portainer:
    image: portainer/portainer
    ports:
      - "9000:9000"
    command: -H unix:///var/run/docker.sock
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data

volumes:
  portainer_data:
```

Once all is up you configure username and password and connect to Docker Service and that's it.

# Application Templates

What I really like in that tool is possibility to use templates to create a docker app directly from UI. You can also define your own template.

![Application Templates](/images/portainer-applications.png)

# Features

All essential docker features are covered by portainer.io, it manage:

- Stacks
- Containers
- Images
- Networks
- Volumes
- Registries

You can also connect to multiple docker engines, or authorize users to use your orchestrator. It could be really useful to see more application template out of the box and some UI improvements.

[portainer.io](https://portainer.io/)