# 📌 DevOps Interview Questions (Organized)

---

# 🧑‍💼 General / Experience

## Questions
Can you summarize your entire career journey in 1–2 minutes?  
How many projects have you worked on? What has your experience been?  
What exactly was your role and responsibility in your project?  
Explain one production issue you handled and how you resolved it.  
Walk me through your project.  
Apart from DevOps, what other technologies or domains have you worked on or learned?  
Do you have any questions for me?  
Tell me about your current project.  

## 💡 How interviewer may ask (Tricky Way)
- Explain your project assuming I am a non-technical person  
- What was your **real contribution vs team work**?  
- What was the **most critical issue** you solved?  
- If I call your manager, what will they say about you?

---

# ☁️ AWS / Cloud

## Questions
Which AWS Application Load Balancer (ALB) metrics do you monitor regularly?  
How do you integrate Corporate security services(Directory) to AWS  
AWM IAM Groups, Roles, and Users related questions  
AWS Route 53 - Explain the different Amazon Route 53 routing policies.  
Two EC2 instances are in private subnets across two different AWS accounts. How would you enable communication between them?  
Why would you choose VPC Peering for this scenario?  
What are the steps involved in configuring VPC Peering?  
Have you worked on VPC Peering in a production environment?  
Have you worked with AWS Client VPN?  
Have you worked with AWS Transit Gateway?  
After creating a Transit Gateway attachment, is that enough for traffic to flow?  
What additional configurations are required after creating a Transit Gateway attachment?  
How would you provide temporary access (2–3 days) to an S3 object?  
How would you share an S3 object with an external customer who does not have an AWS account?  
What is AWS WAF (Web Application Firewall)?  
Why do we use AWS WAF with an Application Load Balancer (ALB)?  
What is Amazon DynamoDB?  
Why would you use DynamoDB instead of a relational database?  
Why is DynamoDB used for Terraform remote state locking?  
Explain the difference between an Application Load Balancer (ALB) and a Network Load Balancer (NLB).  
Which load balancer supports path-based routing?  
Does an Application Load Balancer (ALB) have a static IP address?  
If your application requires a static public IP address, what AWS service or solution would you use?  
How would you provide static IPs for an HTTP/HTTPS application?  
How would you manage user access across multiple AWS accounts?  
What are Permission Sets in AWS IAM Identity Center (AWS SSO)?  
How have you used Permission Sets in your organization?  

## 💡 Tricky Way
- Why NOT use NLB instead of ALB?  
- What happens if Route53 fails?  
- Can you design multi-account access securely?  
- Real-time: **S3 access without IAM user?**

---

# ⚙️ Terraform

## Questions
Have you worked on Terraform?  
What is drift detection in Terraform?  
Lifecycle of Terraform.  
How do you prevent two people from running `terraform apply` at the same time?  
Suppose a state file is ~250MB and it's taking time to load — what's the issue, and how do you mitigate it?  
Explain the Terraform structure of your project.  
How do u provide access to state file?  
State file deleted what do u do?  
Have you worked with Terraform? What are the common issues you have faced while using Terraform?  

## 💡 Tricky Way
- What happens if state file is corrupted?  
- Can Terraform work without state?  
- Real-time: **state locking failure scenario**

---

# 🐳 Docker

## Questions
What is Docker Swarm, Docker Compose, and Podman?  
What are Docker's disadvantages?  
Write a Dockerfile for a Node.js app.  
What networking options are used in Docker?  
Docker- why do we need containers? And what is the use of docker?  
Which Registry u use to store docker images?  
Explain Docker Architecture ,What are the components of Docker Architecture?  
Explain the process of creating a Docker image from a microservice.  
How do you push a Docker image to Amazon ECR?  
What is Docker Compose?  
How do you inspect a running Docker container?  
What does the docker inspect command do?  
What are Docker Image Layers?  
Why does every Docker instruction create a new layer?  
Where are changes stored while a Docker container is running?  
What is the difference between CMD and ENTRYPOINT?  
Which one can be overridden: CMD or ENTRYPOINT?  

## 💡 Tricky Way
- Why containers over VMs in **real production**?  
- What happens if container crashes?  
- Image size optimization?

---

# ☸️ Kubernetes

## Questions
Walk me through K8s architecture.  
What is CrashLoopBackOff? How do you mitigate this issue?  
How do you achieve zero downtime in K8s?  
What is a service mesh?  
What is Istio (Service Mesh)? Why do we use it?  
What is Service Discovery (Self Discovery) in Kubernetes?  
Write a Kubernetes Deployment YAML file including requests, limits, readiness probe, and liveness probe.  
An application is accessible inside the Kubernetes cluster but not from outside. How would you troubleshoot the issue?  
HTTP 503 – Service Unavailable. How would you debug this issue in a production environment?  
Explain the complete request flow from the user to the application.  
Explain Kubernetes Architecture.  
What are the Control Plane components in Kubernetes?  
What are the Worker Node components in Kubernetes?  
What is the role of containerd?  
What is the role of kube-proxy?  
What happens when a container crashes in Kubernetes?  
A Pod is in Pending state. How do you troubleshoot it?  
What are the common reasons for a Pod to remain in Pending state?  
Which command do you use first to troubleshoot a Pending Pod?  
What does kubectl describe pod show?  
What is a Readiness Probe?  
How do you manually scale a Kubernetes Deployment?  
What is Horizontal Pod Autoscaler (HPA)?  
How does HPA work?  
On what metrics does HPA scale Pods?  
What is a Custom Resource Definition (CRD)?  
Why do we need CRDs in Kubernetes?  

## 💡 Tricky Way
- Pod running but app not working — why?  
- Node NotReady — what first check?  
- Difference between **liveness vs readiness (real scenario)**

---

# 🔄 CI/CD & Jenkins

## Questions
Explain your complete CI/CD pipeline process.  
Any issues you've resolved in CI/CD pipelines?  
Declarative vs scripted pipeline  
How jenkins build trigger?  
How do u promote changes to environment (dev, uat, prod)?  
Explain your current CI/CD pipeline. What tools are you using?  
Which CI/CD platform are you using (Jenkins, AWS CodePipeline, or Azure DevOps)?  
Do you have experience writing Groovy scripts?  
Have you created Jenkins pipelines from scratch?  
What type of Jenkins pipeline have you worked on?  
Do you know Jenkins Shared Library?  
Write a Jenkins Declarative Pipeline that performs Checkout, Build, Deploy, and executes post actions  

## 💡 Tricky Way
- Pipeline fails randomly — how debug?  
- Node down scenario?  
- Parallel vs sequential pipeline?

---

# 📦 Git

## Questions
Github merge conflicts resolution steps  
How do u revert the changes in remote repo  
Branching strategies in ur organisation  
What is the difference between Git Merge and Git Rebase?  
Which one do you use in your development environment, Merge or Rebase? Why?  
How do you recover a deleted Git branch?  
What is git reflog?  
How do you recover a deleted commit using git reflog?  
What is git reset --hard?  
What is Git Cherry-pick?  
When do you use Cherry-pick?  

## 💡 Tricky Way
- Rebase gone wrong — recover?  
- Force push mistake — fix?  
- Lost commit recovery?

---

# 📊 Monitoring / Logging

## Questions
Have you set up any monitoring tools such as Prometheus, Grafana, ELK, Kibana, or New Relic from scratch?  
Which monitoring dashboards/tools are you using?  
How are logs shipped to Kibana?  
What is Datadog? Have you worked on it?  

## 💡 Tricky Way
- Alert not triggering — debug  
- Logs missing in Kibana — why?  

---

# 🔥 Real-Time Scenarios

## Questions
Production Scenario: Users report that the application is running slowly. How would you troubleshoot the issue end-to-end?  
A pipeline has failed because one of the build/executor nodes is down. What is your approach?  
A worker container in production is consuming excessive CPU. What steps would you take?  
Walk me through how you would investigate a sudden spike in error logs  
Design a CI/CD pipeline to deploy your microservices application with zero downtime  

## 💡 Tricky Way
- You have NO logs — now what?  
- Everything looks fine but app slow — why?  

---

# 🧠 SRE / Concepts

## Questions
What are the 4 pillars of SRE?  
What is SRE?  
What is GitOps? Explain ArgoCD.  
What is the Hypercare period after deployment?  

## 💡 Tricky Way
- Difference between DevOps & SRE?  
- Why GitOps over CI/CD?

---

# 🐧 Linux

## Questions
How do you monitor context switches in Linux? Which commands do you use?  
In the vm we need to get particular file from root level - which commands do you use in linux ?  
For suppose in file i want to fetch error or exception with line numbers I need to get.  

## 💡 Tricky Way
- Server slow — first 3 commands?  
- CPU high — debug steps?

---
===================================================
# 📌 DevOps / AWS / Kubernetes Interview Questions (Organized)

---

# 🧑‍💼 Introduction / General

## Questions
Tell me about yourself.  
Tell me about yourself.  
Can you introduce yourself?  

## 💡 How interviewer may ask (Tricky Way)
- Tell me about yourself in 1–2 minutes  
- Tell me something not mentioned in your resume  
- What makes you different from other candidates?  

---

# ☁️ AWS Basics & Architecture

## Questions
What is the difference between On-Demand Instances and Spot Instances? (for me No idea )  
Design a highly available three-tier architecture in AWS.  
Which AWS services would you use for a three-tier application?  
Why do we use a VPC?  
Why do we use multiple Availability Zones?  
What is the purpose of an Internet Gateway?  
What is the purpose of a NAT Gateway?  
What is the purpose of an Application Load Balancer (ALB)?  
Why do we use Amazon Route 53?  
Why do we use IAM Roles?  
Why do we use AWS Secrets Manager?  
What is the use of Amazon CloudWatch?  
What is the use of AWS CloudTrail?  
How does Auto Scaling work?  
When do you choose Amazon EC2 over Amazon EKS?  
What type of applications do you deploy on Amazon EKS?  
What type of applications do you deploy on EC2?  
How do you create an Application Load Balancer (ALB)?  
How do you get the DNS URL of an ALB?  
How do two subnets communicate within the same VPC?  
What is the difference between an Internet Gateway and a NAT Gateway?  

## 💡 Tricky Way
- What happens if one Availability Zone goes down?  
- Can private subnet resources access the internet? How?  
- Design a secure architecture without exposing servers publicly  
- Why not use NLB instead of ALB?  

---

# ☸️ Kubernetes / EKS

## Questions
Explain your Kubernetes experience in your project.  
What were your responsibilities while working with Kubernetes?  
Which Kubernetes resources have you worked with?  
Which Kubernetes commands do you use regularly?  
Explain the Kubernetes deployment flow in your project.  
How do you deploy applications to Amazon EKS?  
What is CrashLoopBackOff?  
What are the common reasons for CrashLoopBackOff?  
How do you troubleshoot a CrashLoopBackOff issue?  
What AWS services have you worked on?  
Can you explain your experience with Amazon EKS?  
What kind of applications have you deployed in EKS?  
How do you deploy an application in EKS?  
Which Kubernetes resources do you use while deploying an application?  
How is your EKS network architecture designed?  
Why are your worker nodes in private subnets?  
Why is the Load Balancer placed in a public subnet?  
How do developers connect to the EKS cluster?  
How does Jenkins connect to the EKS cluster?  
Which commands do you use to connect to EKS?  

## 💡 Tricky Way
- Pod is running but application is not accessible — why?  
- CrashLoopBackOff but no logs — what will you do?  
- Why keep worker nodes private? What happens if public?  
- How kubectl communicates with cluster internally?  

---

# ⚖️ Autoscaling (Kubernetes)

## Questions
Have you configured Horizontal Pod Autoscaler (HPA)?  
How does HPA work?  
On which metrics does HPA scale the pods?  
Have you configured Cluster Autoscaler?  
How does Cluster Autoscaler work?  
What is the difference between HPA and Cluster Autoscaler?  
Explain the Min, Desired, and Max node settings in an EKS node group.  

## 💡 Tricky Way
- Pods are scaling but nodes are not scaling — why?  
- CPU usage is low but pods are scaling — reason?  
- Difference between manual scaling and auto scaling in real-time  

---

# 🌐 Networking / Request Flow

## Questions
Explain the complete request flow from the client to the Kubernetes pod.  
Which Load Balancer do you use in your project?  
Why do you use an Application Load Balancer (ALB)?  
When would you use a Network Load Balancer (NLB)?  

## 💡 Tricky Way
- Explain request flow in under 1 minute  
- Where can latency occur in this flow?  
- Where does SSL/TLS termination happen?  

---

# 🔄 CI/CD (Jenkins)

## Questions
How did you deploy infrastructure changes through the CI/CD pipeline?  
Explain your CI/CD deployment process.  
What happens after developers push the code?  

## 💡 Tricky Way
- Pipeline succeeded but deployment failed — why?  
- How do you rollback a failed deployment?  
- How do you achieve zero downtime deployment?  

---

# ⚙️ Terraform

## Questions
Explain your Terraform project structure.  
What Terraform modules have you worked on?  
What was your role in Terraform infrastructure deployment?  
Which AWS resources did you provision using Terraform?  
How did you deploy infrastructure changes through the CI/CD pipeline?  
Explain explicit and implicit dependencies in Terraform.  
How do you manage dependencies between AWS resources in Terraform?  
If each EC2 instance requires a different name or configuration, how would you create them in Terraform instead of using count?  

## 💡 Tricky Way
- count vs for_each — when to use?  
- What happens if dependency fails?  
- How do you handle state mismatch issues?  

---

# 🚨 Troubleshooting / Production Issues

## Questions
If the application is not accessible, how do you troubleshoot it?  
What logs and metrics do you check during troubleshooting?  
Which CloudWatch metrics do you use for troubleshooting?  
If a Jenkins deployment fails, how do you identify the root cause?  
Which Jenkins logs do you check first?  
Before redeploying after a failure, what steps do you follow?  
How do you identify and fix the root cause?  

## 💡 Tricky Way
- No logs available — how do you debug?  
- Everything looks fine but app is slow/down — why?  
- Pipeline success but app not working — root cause?  

---

# 📌 Focus Area

They focus primarily on AWS, Kubernetes, Terraform, EKS, Networking, CI/CD, and DevOps project experience.
