## k8s-spot-handler

A Kubernetes DaemonSet to run 1 container per node to periodically polls the [EC2 Spot Instance Termination Notices](https://aws.amazon.com/blogs/aws/new-ec2-spot-instance-termination-notices/) endpoint.
Once a termination notice is received, it will try to gracefully stop all the pods running on the Kubernetes node, up to 2 minutes before the EC2 Spot Instance backing the node is terminated.

## Installation

### Kubernetes templates

```bash
kubectl apply -f sample-deploy-k8s/*
```

## Available docker images/tags

Tags denotes Kubernetes/`kubectl` versions.
Using the same version for your Kubernetes cluster and `k8s-spot-handler` is recommended.
Note that the `-1` (or similar) is the revision of this tool, in case we need versioning.

* `marcincuber/k8s-spot-handler:1.12.8-1`
* `marcincuber/k8s-spot-handler:1.13.6-1`
* `marcincuber/k8s-spot-handler:1.14.2-1`

## Why use it

  * So that your kubernetes jobs backed by spot instances can keep running on another instances (typically on-demand instances)

## How it works

Each `spot-termination-notice-handler` pod polls the notice endpoint until it returns a http status `200`.
That status means a termination is scheduled for the EC2 spot instance running the handler pod, according to [my study](https://gist.github.com/mumoshu/f7f55e6e74aaf54f63d263326ca58ba3)).

Run `kubectl logs` against the handler pod to watch how it works.

```
$ kubectl logs --namespace kube-system k8s-spot-handler-ibyo6
This script polls the "EC2 Spot Instance Termination Notices" endpoint to gracefully stop and then reschedule all the pods running on this Kubernetes node, up to 2 minutes before the EC2 Spot Instance backing the node is terminated.
See https://aws.amazon.com/jp/blogs/aws/new-ec2-spot-instance-termination-notices/ for more information.
`kubectl drain minikubevm` will be executed once a termination notice is made.
Polling http://169.254.169.254/latest/meta-data/spot/termination-time every 5 second(s)
Fri Jul 29 07:38:59 UTC 2016: 404
Fri Jul 29 07:39:04 UTC 2016: 404
Fri Jul 29 07:39:09 UTC 2016: 404
Fri Jul 29 07:39:14 UTC 2016: 404
...
Fri Jul 29 hh:mm:ss UTC 2016: 200
```

## Author contact

* Name: Marcin Cuber
* Email: marcincuber@hotmail.com
