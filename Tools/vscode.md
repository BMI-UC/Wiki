---
title: VS code
---

[Visual Studio Code](https://code.visualstudio.com/) is a text editor that can
be configured to write almost any language.

## Connecting to the cluster

- Install the
  [Remote ssh extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)

  - This extension allows you to connect to remote hosts (like the cluster) via
    ssh You can use the remote extension to connect your local vscode with the
    cluster.

- Recommended: create an ssh config

![](./vscode/get-to-ssh-config.mp4)

If you want to connect to the cluster outside of a vpn, you'll need your config
to look something like this:

```
Host vscode-cluster
    HostName bmiclusterp1.chmcres.cchmc.org
    User USERNAME
    ProxyJump USERNAME@ssh.research.cchmc.org
```

This config first connects to `USERNAME@ssh.research.cchmc.org`, then connects
to `USERNAME@bmiclusterp1.chmcres.cchmc.org` inside `ssh.research.cchmc.org`
session.

This will also allow you to use `ssh vscode-cluster` in the command line to get
the same environment as vscode.

Otherwise, if you'll be connected to the vpn, you can simply connect to the p1
cluster:

```
USERNAME@bmiclusterp1.chmcres.cchmc.org
```

### Why is the 1 needed?

- If you are used to connecting via ssh, you'll notice that the normal
  `bmiclusterp.chmcres.cchmc.org` address is changed to
  `bmiclusterp1.chmcres.cchmc.org`
- The head nodes on the p2 and p3 clusters, which are the default, are on CentOS
  7, a very old, unsupported operating system
- The compute nodes are on RHEL 9, a fairly new operating system
- vscode is incompatible with CentOS 7, but citrix, an old way to connect to the
  cluster, is only supported by the old head nodes, not the new RHEL 9 nodes.
- p1 head nodes are on RHEL 9, so vscode can connect

### Supplementary material

- [CCHMC's ssh instructions](https://hpc.research.cchmc.org/placeholder)
