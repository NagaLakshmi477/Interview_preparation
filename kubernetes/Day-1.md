
1. Did you recently create a Kubernetes Deployment and Service? Why did you create them, and what purpose did they serve?

We use Deployment instead of creating a Pod directly because if a Pod crashes or gets deleted, the application can go down.

When we create a Deployment, it creates and manages a ReplicaSet. The ReplicaSet maintains the required number of Pods.

For example, if we have 3 replicas and one Pod crashes, the ReplicaSet automatically creates a new Pod. So we don't need to manually create the Pod again.

That's why we use Deployment — to make sure the required number of Pods are always running.

For Service, we use it because every Pod has its own IP address, and Pod IP addresses can change when Pods are deleted and recreated.

So instead of connecting to the Pod directly using its IP address, we use a Service. The Service provides a stable endpoint and forwards the traffic to the available Pods.

So simply:

Deployment → manages and maintains the Pods.

Service → provides stable communication to the Pods.

2. can you explain what is deployment ?

Deployment is a Kubernetes object used to manage and maintain the Pods of an application.

For example, if I have a catalogue application and I want 3 Pods to run, I can create a Deployment with 3 replicas.

The Deployment creates and manages a ReplicaSet, and the ReplicaSet makes sure the required number of Pods are always running.

If one Pod crashes or gets deleted, the ReplicaSet automatically creates a new Pod.

So, the main purpose of Deployment is to make sure my application Pods are running as required and to maintain the desired number of replicas.

3. if pod is down how do you debug ?

If a Pod is down, first I check the Pod status using kubectl get pods.

After that, I use kubectl describe pod to check the Pod details and especially the Events section, because it can tell me why the Pod is not running.

Then I check the Pod logs using kubectl logs to understand whether there is any application-level error.

If the container has already crashed and restarted, I also check the previous logs using kubectl logs --previous.

If required, I check the Deployment and ReplicaSet to make sure they are creating and maintaining the required number of Pods.

So my debugging approach is:

First check the status → then describe the Pod → check Events → check logs → check previous logs if required → then check Deployment and ReplicaSet.

CI/CD / GitHub Actions / Reusable Workflows



I would use package when I only need the build artifact, and I would use install when another local Maven project needs to use that artifact as a dependency.
