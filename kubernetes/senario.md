## what are the some challenges with promotheus

despite of very good at k8s promotheus still have some issues



challanges
==============
## resource sharing
- How you can allocates resources onmultiple envi(dev,prod,QA)
As a devops engineer when joining on a organization there was a cluster that shared across all environments
beacuse of one of pod leaking memory entire cluster was impacted so we don't know which pod is going down and which namespaces
beacuse of out of memory as a devops engineer i ahve setup the resouce quota and resource limit on the pods of the particular namespace
beacuse of which i can identify which pod is 
or
As a DevOps engineer, when I joined an organization, there was a Kubernetes cluster shared across multiple environments like Dev, QA, and Prod.

One day, one of the Pods had a memory leak and started consuming more memory. Because there were no proper resource limits, it impacted the entire cluster, and it was difficult to identify which Pod or namespace was consuming more resources.

So, as a DevOps engineer, I set up ResourceQuota for each namespace and configured resource requests and limits for the Pods.

Because of this, we were able to control how much CPU and memory each namespace and Pod could consume. If any Pod started consuming excessive resources, it would not impact the entire cluster, and we could easily identify which Pod or namespace was causing the issue.

- OOM killed issue with POD
so after setting the resuorce quota you know that one pod is getting OOM killed which is a crashloopbackoff
so what i have was this is java based micro service so i login into this pod and shared head dump and tread dump and shared those to developer team
or
If a Pod is getting OOMKilled, first I identify which Pod is consuming more memory.

In my case, after setting the resource quotas and resource limits, I identified that one of the Pods was getting OOMKilled and going into CrashLoopBackOff.

This was a Java-based microservice. So, I checked the Pod and collected the Java heap dump and thread dump to understand why the application was consuming more memory.

I shared the heap dump and thread dump with the development team so they could analyze the application and identify the memory leak or memory-related issue.

Based on their analysis, we worked on fixing the issue and then monitored the Pod to make sure it was running properly without getting OOMKilled again.

- Upgrades

For Kubernetes upgrades, we prepared a very detailed manual with step-by-step procedures.

Before performing the upgrade, I documented how to take the required backups and how to check the Kubernetes release notes and upgrade requirements.

I divided the upgrade process into two main parts: the control plane components and the worker nodes.

For the control plane, I documented the order of the components to be upgraded and how to verify the health of important components like `etcd` and the API server before and after the upgrade.

For the worker nodes, before upgrading a node, I first drain the node so that the workloads are safely moved to other available nodes. Then I perform the upgrade and verify that the node comes back to the `Ready` state.

After upgrading all the nodes, I test the cluster and verify that all the Pods, Services, and applications are working properly.

We documented the complete process in a manual so that the upgrade could be performed in a controlled way and the same steps could be followed by other team members as well.
