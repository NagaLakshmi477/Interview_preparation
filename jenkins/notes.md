Jenkins, Docker Agent & Argo CD Notes

Jenkins

For Jenkins, we need Java, because Jenkins is a Java-based application.

So first we need to install a supported Java/JDK version, and then we can install Jenkins.

Jenkins Controller and Nodes

Previously, we used a Jenkins Controller with multiple nodes/agents.

Jenkins Controller
        |
        +---- Linux Agent
        +---- Ubuntu Agent
        +---- Windows Agent

We can use different types of operating systems depending on the application requirement.

For example, if we have a Windows agent and it is used only occasionally, the EC2 instance may remain idle most of the time.

So we are wasting EC2 resources.

Jenkins with Docker Agents

Instead of maintaining dedicated EC2 instances as agents, we can use Docker containers as Jenkins agents.

Jenkins Controller
        |
    Docker Engine
        |
        +---- Docker Container
        +---- Docker Container
        +---- Docker Container

If a pipeline needs a particular environment, Jenkins can use a Docker container to execute the pipeline work.

For example, if we need Node.js, we can use a Node.js Docker image.

The container can be created when the pipeline needs it, and after the work is completed, the container can be removed.

This helps us avoid keeping dedicated EC2 agents running all the time.

Docker containers are lightweight compared to full EC2 instances.

Multi-Stage and Multi-Agent

A pipeline can have multiple stages:

Pipeline
   |
   +---- Checkout
   |
   +---- Build
   |
   +---- Test
   |
   +---- Deploy

A stage is a logical step in the pipeline.

Different stages can also run on different agents.

Build  → Agent 1
Test   → Agent 2
Deploy → Agent 3

So:

Stage = logical step

Agent = where the pipeline work is executed

Argo CD

Argo CD is a GitOps-based continuous delivery tool for Kubernetes.

Argo CD continuously monitors the desired application configuration stored in Git.

It compares:

Desired State in Git
        VS
Actual State in Kubernetes

If there is a difference, Argo CD can synchronize the Kubernetes cluster with Git, depending on the configured sync policy.

Developer
    |
    v
  GitHub
    |
    v
  Jenkins
    |
    +---- Build
    +---- Test
    +---- Scan
    +---- Build Docker Image
    +---- Push Image
    |
    v
Git deployment configuration
    |
    v
  Argo CD
    |
    v
Kubernetes / EKS

Simple Understanding

Jenkins → CI

Build, test, scan, create Docker image and push the image.

Docker → Agent

Docker containers can be used as lightweight Jenkins agents to execute pipeline work.

Argo CD → CD / GitOps

Argo CD monitors Git and synchronizes the desired application state with Kubernetes.
