#### By definition its a container orchestration technology that automates the deployment, scaling, and management of containerized applications - but what does that actually mean.

## Why we need kubernetes?

Traditionally, applications were deployed on [[Virtual Machines]] (VMs). Depending on the workload, a VM often hosted one or a small number of applications. Each VM had its own operating system (for example, Ubuntu), which meant every VM consumed CPU, memory, and storage just to run the OS. Then [[Docker]] came into picture and popularized the concept of containerization, people started containerizing their applications, the advantage of docker was that you don't need an entire Operating System to run your application, you can just containerize it and can have multiple containers running at the same time on the same VM, Since containers share the host operating system, you can run many more applications on the same machine. This leads to much better resource utilization and often reduces the number of VMs you need.

## But how do we route traffic using docker?

If we want to have multiple replicas of the application to route the traffic because a [[container]] can process only so many requests, in that case we need to have multiple of these containers and once a VM run out of CPU/RAM you need to crate more VM's and more containers in that VM.

The containers running on the VM's are not automatically aware of each other.

Now let's say we have 4 VM's and each has some docker containers running on them (that's basically our applications running on each container) how do we route the traffic? 

So to route the traffic we can use load balancers e.g.- HAPoxy it'll get the request from the outside and using round robin fashion and route the requests to available container, now this was scalable but it was a lot of manual work, moreover the container running on x VM did not had any idea of the containers on other VM's

This is where kubernetes comes in - you can say it is a OS of the cloud - k8s is software that runs on a cluster of machines which makes them able to communicate properly to each other and share the work load

Under the  hood it's still a bunch VM's but it has a [[control plane]] which is connected to this VM's these VM's are called as [[worker nodes]] 

Kubernetes uses [[YAML]] files to describe the desired state of your applications. For example, you can declare that your application should always have three replicas. Kubernetes continuously works to make reality match that desired state. so let's say i want to have 3 replicas of my application([[Image]]) I'll declare so in my YAML fine then control plane will take it as set of commands and create 3 replicas for you

Now some applications are really big and may need a large number of containers(say 100) - k8s can also calculate and scale up the nodes required to run these containers each node has limited CPU and memory, so it can only run a certain number of [[Pods]] (or containers). If the existing nodes don't have enough capacity, Kubernetes can schedule Pods onto other nodes. When used with a Cluster Autoscaler, it can even provision additional nodes automatically.

k8s maintains a desired state and actual state if i said i want 3 replicas and right now i only have 2 it'll spin up a new one for me, so the desired and actual state matches.

containers sometimes crash due to various reasons but k8s make sure that there's always desired numbers of replicas running takes down the crashed one and spins up a new one this way we always have desired number of containers/pods running.

Kubernetes also supports rolling updates. Instead of stopping the entire application during a deployment, it gradually replaces old containers with new ones, allowing updates with little or no downtime.

### Conclusion - 

Imagine you have a web application running in a Docker container.

Running one container is simple:

```
Docker → Run container
```

But what if you need:

- 100 containers instead of 1?
- Automatic recovery if a server crashes?
- More containers during peak traffic?
- Zero-downtime updates?
- Load balancing across multiple servers?

Managing all of that manually is difficult. Kubernetes automates it.