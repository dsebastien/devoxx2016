# Managing Cloud Native apps with Kubernetes
* Software engineers -> Control Plane <-> Datacenter

## Linux Application Containers
* cgroups, namespaces, capabilities, chroots
* good fences make good neighbours

## At google
* Batch workloads
  * Global Work Queue
* Production workloads
  * Babysitter

Both are hosted on "Borg Cell", running on the same machines

They run multiple applications per machine to maximize hardware utilization

Sharing clusters between prod/batch helps
* segregating them would need more machines
* they gain between 15-20% per machine

For a given cell, they define a (static) limit: amount of resource requested that can be accepted
* reservation: estimate of future usage
* usage: actual resource consumption

Task-eviction rates and causes:
* production jobs don't get often evicted
* happens more with batch jobs (e.g., machine failure, machine shutdown, out of resources, etc)

Advanced bin-packing algorithms
* they optimize the cpu and memory usage on machines (e.g., avoid situations where there is still memory usable but no cpu available or the opposite, etc)

Observations
* efficiency comes from
  * scavenging unused allocations
  * effective prioritization
  * sharing resources
  * overcommit
  * smarter scheduling
* application-centric ...
...

## Containers recap
* lightweight
* hermetically sealed
* isolated
* easily deployable
* introspectable
* runnable
* linux processes
* improves overall developer experience
* fosters code and component reuse
* simplifies operations for cloud native applications

# Managing containers
Required as there are more and more containers to manage. There is a need for a scheduler.

What we want
* Developers -> API <-> Datacenter | Amazon web services | Google Cloud platform | On premise | Raspberry Pi cluster | ...

# Kubernetes
An ocean of containers... scheduled and packed dynamically onto nodes by the Kubernetes Master

Quick intro
* scheduling: decide where the containers should run
* lifecycle and health: keep containers running despite failures
* scaling: make sets of containers bigger or smaller
* naming and discovery: find where my containers are now
* load balancing: distribute traffic across a set of containers
* storage volumes: provide data to containers
* logging and monitoring: track what's happening with my containers
* debugging and introspection: enter or attach to containers
* identity and authorization: control who can do things with my containers

Kubernetes
* takes in container packaged apps and emits microservices architectures
* 1.5 due soon
* stewarded by the Cloud Native Compute Foundation
* stable
  * concrete ideas from 10 years of production experience
  * v1 API, breaking changes held until v2
  * alpha, beta and GA tracks
  * thorough end-to-end testing
  * new work taking place outside of core
    * volume & network plugins
    * custom controllers
    * ...
  * solid core
  * large ecosystem: cloud providers, distros, paas, cd, deployment, ...

Looks like
* Developers -> API / CLI / UI => Kubernetes Cluster (Control Plane <-> Servers (Kubelets, ...))

## Getting started
Start with a cluster:
* laptop or HA multi-node cluster
* hosted or self-managed
* on premise or cloud
* bare metal or virtual machines
* supported on most OSes
* ...

## Setting up a cluster
* linux on premise: create nodes, run kubeadm to configure master and nodes
* AWS: run Kubernetes Operations (KOPS)
* GKE: spin up a cluster from the console or from gcloud
* choose a platform: GCE, AWS, Azure, Rackspace, Ubuntu, Juju, ...
  * then run `export KUBERNETES_PROVIDER=<your_provider>; curl -sS https://get.k8s.io | bash`
* choose a distro such as RedHat Atomic, CoreOS Tectonic, Mesos, Murano (OpenStack), ...
* build from scratch (check out Kubernetes the Hard Way): https://github.com/kelseyhightower/kubernetes-the-hard-way

## Pods
Containers are wrapped inside pods

A pod is:
* the atom of scheduling for containers
* represents an application specific logical host
* each pod has its own routable (no NAT) IP address
* supports liveness and readiness checks
* postStart and preStop lifecycle hooks

Why pods?
* allow for support of multiple container formats
* simplify networking
* allow us to group containers

Containers within a pod
* share port and IPC namespaces
* talk to each other through localhost

Pod networking rules
* pods have IPs which are routable
* can reach other without NAT (even across nodes)
* no brokering of port numbers

Pods are built from a spec template and can be replicated. Replicas are functionally identical and therefore ephemeral and replaceable

## Labels
Grouping mechanism in Kubernetes.
Labels are key-value pairs that can be attached to anything in Kubernetes

E.g., Pod with type=FE (front-end)
E.g., Pod with app=demo (demo application element)

Make it possible to build queries for elements based on their labels

* metadata with semantic meaning -> allow for intent of many users
* membership identifier -> build higher level systems
* the only grouping mechanism -> queryable by selectors

## ReplicaSets
* keeps pods running -> recreates pods, maintains desired state
* gives direct control of pod #s -> fine-grained control for scaling
* grouped by label selector -> standard grouping semantics

Make sure that pods are running

## Deployments
Updates as a service
* reliable mechanism for creating, updating and managing pods
* deployment manages replica changes, including rolling updates and scaling
* edit deployment configurations in place with `kubectl edit` or `kubectl apply`
* managed rollouts and rollbacks

In beta since Kubernetes 1.2

## Services
* pods can run anywhere within the cluster
* pods are ephemeral (their IPs are NOT stable)
* we can have many replicas of the same pod

How do pods find each other?
How do external services discover them?
How do we route traffic to pods?

Services are
* a logical grouping of pod replicas
  * grouped by label selector
  * pods run the same app but may be different versions (canary)
* load balances incoming requests across constituent endpoinds (pods)
* choice of pod is random but supports session affinity (ClientIP)
* gets a stable virtual IP and port (also a DNS name)

## External services
Services VIPs are only available inside the cluster, but we need to receive traffic from the outside worlds

Service "type"
* NodePort: expose on a port on every node
* LoadBalancer: provision a cloud load-balancer

DiY load-balancer solutions
* socat (for nodePort remapping)
* haproxy
* nginx

Ingress (Layer 7 load balancer)

## Ingress for HTTP load balancing (beta)
Allow to expose a given DNS/IP for a given service, being load balanced between different parts

## Scaling
The ReplicaSet configuration defines how many pods should be kept up and running

## Rollout
What happens with deployments: API -> Deployment -> create ReplicaSet -> create pods
* `kubectl edit deployment ...`

We can implement complex scenarios:
* have already a ReplicaSet with v1 of the app (with 2 pods)
* change the rollout to create a new ReplicatSet with v2 of the app (but 0 pods)
* remove progressively pods of v1 and add v2 pods

## Canary
Multiple ReplicaSets with different versions, but all load balanced. Progressively introduce new version

## Pod horizontal auto-scaling
...

## Kubernetes without a Scheduler
Create a pod with a given spec, we can force the node where that pod should be executed

## Kubernetes Scheduler
Create a pod with a spec, but we don't specify on which node it should run. Kubernetes Scheduler takes care of the scheduling: looks at the environment, the available resources, the currently running containers, etc

## Volume
Volumes are
* bound to the pod that enclose it
* look like a directory to containers
* what, where and what it contains is determined by Volume Type
* live as long as the pod does

Volume types
* EmptyDir: lives with the pod
* HostPath: maps to directory on host; use with caution!
* nfs (and similar services)
* cloud provider block/file storage
* PersistentVolumeClaim

## PersistentVolumes
* Higher level storage abstraction: insulation from any one cloud environment
* Admin provisions them, users claim them (new: Dynamic Provisioning in 1.4)
* Independent lifetime from their consumers
* ...

## Clustered Applications
...

## Pet Sets (alpha)
A Pet Set ensures that a specified number of "pets" with unique identities are running at any given time.

The identity of a Pet is comprised of
* a stable hostname, available in DNS
* an ordinal index
* stable storage: linked to the ordinal & hostname

Allows to bring up different nodes in a cluster with each having its own identity & storage.

Useful to run stateful apps in Kubernetes

## ConfigMaps
Goal: manage app configuration without making overly brittle container images

12-factors syas config comes from the environment: Kubernetes is the environment

Manages secrets via the Kubernetes API

Inject them as virtual volumes into Pods
* late-binding
* never touches the physical disk (more secure)

## demo
* UI
* 2 back-end REST APIs
* Redis for session replication

Packaging & deployment: don't tie one application to one environment / one machine
* instead make it environment agnostic & not relying on specific machine configuration
* containerize the apps
* deploy the smallest thing possible: don't create large wars / ear files mixing concerns
  * by deploying separately, we can scale out each parts in isolation

## Maven
* com.spotify:docker-maven-plugin:0.2.11
* configuration: baseImage, imageName, imageTags, forceTags, entryPoint, ...

## Onbuild dockerfile
... ONBUILD ...
* whenever the image is built, the commands specified in ONBUILD lines will be executed
* means that these are not executed when the containers are spinned up! (cool to handle compilation, ...)

## demo
* docker run -ti helloworld:devoxxw16
* docker tag helloworld:devoxx16 gcr.io/wise-coyote-827/helloworld-service:devoxx16 (private docker registry on google cloud platform)

Deployment
* easy way: kubectl run helloworld-service --image=gcr.io/wise-coyote-827... -l app=helloworld-service,version=devoxx2016,env=staging,conf=dev,visualize=true...
  * passing labels is super useful

Scaling
* kubectl scale deployment helloworld-service --replicas=4
  * will scale up to 4 pods

Expose
* kubectl edit configmap exposecontroller
* kubectl expose deployment/helloworld-service --target-port=8080 --port=8080
  * creatse a network load balancer (level 4) for all pods of the hello-service
  * downside: clients will remain connected to the same pod for as long as it wants, so load balancing is not intelligent

To load balance more intelligently, we need a layer 7 load balancer, that we can do using ingress.

Debugging is easy
* kubectl get pods
* kubectl logs -f helloworld-service-...
  * in the past they outputted to log files but that fills disks, requires rotation, etc
  * in Kubernetes we can direct to std out
  * on Google Cloud Platform they collect all those logs and centralize them

Get into a container
* kubectl exec -ti helloworld-service... /bin/bash

Service discovery
* server side: http://helloworld-service:8080/hello/Ray
  * handled automagically by Kubernetes
  * localhost:8080/api/v1/endpoints|services|...
    * API exposed by Kubernetes

Infrastructure as code
* checking in the infrastructure configuration files/descriptors
* yaml (or json) files describing the application
  * if not provided, Kubernetes actually creates one itself based on user input on the CLI
  ```
    kind: Service|LoadBalancer|...
    ...
    metadata:
        name: helloworld-service
        labels:
            app: helloworld-service
            visualize: true
    ...
  ```

## Health checks
In the infrastructure descriptor, we can add `livenessProbe` to configure health checks
Example: execute a script, do an http get, ...

```
    livenessProbe:
        httpGet:
            path: ...
```

## Cancelling/Reverting deployments
If there's a wrong config, the rollout will be paused. We can undo it easily using `kubectl rollout undo deployment helloworld-ui`

`kubectl rollout history deployment helloworld-ui`: we can rollback easily

## Configuration
We can define an `env` key in the application configuration file

```
   ...
   envs
```