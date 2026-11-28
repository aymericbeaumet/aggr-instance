---
title: 'Tabernetes: A Kubernetes cluster whose nodes are browser tabs'
link: https://tabernet.es/
source: hnrss-org
published: 2026-11-27T23:00:30Z
updated: 2026-11-27T23:00:30Z
first_seen: 2026-11-28T14:03:39.806524745Z
authors:
- mmcclure
content: extracted
html: 2026-11-27-tabernetes-a-kubernetes-cluster-whose-nodes-are-browser-tabs.html
remote_preview:
  url: https://tabernet.es/card.png
  alt: 'tabernetes: a Kubernetes cluster whose nodes are browser tabs, beside a globe of dots with lines from phones to the cluster'
---

Open the page and your phone is a node. The scheduler sends it a pod, and the pod's program runs on a Linux kernel booted in the tab. Close the tab and the node is gone, and Kubernetes starts the pod on someone else's.

![A QR code for the page that makes a browser a node](https://tabernet.es/qr.png)

**Scan to become a node**, or use the browser you are reading this in.

[Join the cluster](https://tabernet.es/join)

Joining downloads about 5 MB and runs a small Linux guest in the tab, on one core, until you close the tab.

0

nodes now

0

pods running in tabs

0

countries

-

median boot

-

cluster age

0

hours of tabs lent

0

pods moved off a closed tab

0

carried between tabs and the machine

## Topology

The control plane in the middle, a node for each tab, and each Service on the way to the pods behind it. A node is filled when its pod is running. An orange dot is a request you sent, on its way to the pod that answered, and a green one is a tile of the picture on its way to the node that rendered it.

## Boot time

How long a tab takes to start its pod: from the tab being handed the pod to the pod's program running, which is the page loading, the kernel booting and the image being mounted. The last 0 starts.

No pod has started yet.

## Happening now

Nothing yet. A tab that joins shows up here.

## The fleet

What the nodes are, by what their browsers say.

## The nodes

| Node | Device | Browser | Near | Pod | Boot | Carried | Age |
| ---- | ------ | ------- | ---- | --- | ---- | ------- | --- |

No tab is a node right now. Yours can be the first.

## How it works

1. **A tab joins.** The join page holds a socket open to an agent beside the API server, and the agent registers a Node for the tab.
2. **The scheduler sends a pod.** The agent starts the pod's container with [tabc](https://github.com/fzakaria/tabc), a container runtime whose containers run in browser tabs.
3. **The tab boots Linux.** A kernel compiled to WebAssembly runs the pod's program on the tab's CPU and memory. The pod's files and its address stay on the cluster's machine.

## A pod asks for a tab in one line

The control plane is an ordinary Kubernetes, and a pod is an ordinary pod. The marked line is the only one that is new: the scheduler puts a pod that has it on a tab, and every other pod on the cluster's own machine.

```
apiVersion: v1
kind: Pod
metadata:
  name: hello
spec:
  runtimeClassName: tab
  containers:
    - name: hello
      image: alpine
      command: ["sh", "-c", "echo hello from $(uname -m) in a tab; sleep 3600"]
```
