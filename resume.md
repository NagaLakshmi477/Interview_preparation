# Tell Me About Yourself

I’m a **DevOps Engineer with 4+ years of experience** in AWS, CI/CD, Docker, Kubernetes, Terraform, and Ansible, with a strong focus on automation and cloud infrastructure.

Currently, I’m working at **HTC Global Services**, where I’m mainly responsible for **CI/CD automation, containerization, Kubernetes deployments, AWS infrastructure, and monitoring**.

In my current project, we have an end-to-end CI/CD process where developers push code to **GitHub**, which triggers **Jenkins**. Jenkins performs the build process, followed by **SonarQube code-quality analysis and Quality Gate checks**. We also perform **security and vulnerability scanning** as part of our **Shift-Left DevSecOps approach**.

Once the checks are successful, we build a **Docker image**, tag it appropriately, and push it to **Amazon ECR**. From there, we deploy the application to **Amazon EKS using Helm**. I’ve also worked with **blue-green deployments, Kubernetes scaling, storage, and troubleshooting**.

For monitoring, I’ve worked with **Prometheus and Grafana** to monitor Kubernetes workloads and application metrics and to create dashboards and alerts.

I also have experience with **Terraform** for infrastructure provisioning and **Ansible** for configuration management and application deployment. On AWS, I’ve worked with services such as **EC2, VPC, ALB, Route 53, NAT Gateway, S3, ECR, and EKS**.

Before joining HTC Global Services, I worked at **Deloitte**, where I primarily worked on **AWS infrastructure using Terraform and application deployment using Ansible**.

Earlier in my career, I worked as a **Python Developer**. Later, I became interested in cloud technologies, automation, and infrastructure, which led me to transition into DevOps.

I’m also certified in **Microsoft Azure Fundamentals, AZ-900**.

# Explain Your Current Project

Currently, I’m working as a **DevOps Engineer at HTC Global Services**. My project involves automating the application deployment process and managing the cloud infrastructure and Kubernetes environment.

The application is hosted on **AWS**, and we use **Amazon EKS** as our Kubernetes platform. My main responsibility is to manage the **CI/CD pipeline, infrastructure automation, containerization, Kubernetes deployments, and monitoring**.

The deployment process starts when a developer pushes code to **GitHub**. A **GitHub webhook triggers Jenkins**, which starts the CI/CD pipeline.

In Jenkins, we first perform the **build process**, followed by **SonarQube code-quality analysis and Quality Gate validation**. As part of our **Shift-Left DevSecOps approach**, we also perform security and vulnerability scanning before deploying the application.

After all the checks are successful, we build the application into a **Docker image** and push the image to **Amazon ECR**.

For deployment, we use **Helm charts** to deploy the application into **Amazon EKS**. Helm helps us manage Kubernetes resources such as Deployments, Services, ConfigMaps, and other application configurations.

I’ve also worked with **blue-green deployments**, where we maintain the existing version while deploying the new version separately. After validating the new version, we switch the traffic to the new version, which helps reduce downtime and makes the deployment safer.

For infrastructure provisioning and automation, I use **Terraform**. We use Terraform to provision and manage AWS resources such as **VPC, EC2, ALB, EKS, IAM, S3, and other required infrastructure**.

I have also worked with **Ansible** for configuration management and application deployment-related tasks.

For monitoring, we use **Prometheus and Grafana**. Prometheus collects metrics from the Kubernetes environment, and Grafana is used to create dashboards and monitor application and infrastructure health. We also use alerts to identify issues such as high resource utilization or unhealthy workloads.

So, overall, my role in the project is mainly focused on **end-to-end CI/CD automation, AWS infrastructure, Docker and Kubernetes deployments, DevSecOps practices, and monitoring**, with the goal of making deployments more reliable, automated, and scalable.

# Can You Explain Your Complete CI/CD Flow?

Sure. In my project, we use **Jenkins** to automate the complete CI/CD process, from code integration to application deployment and monitoring.

First, the **developer pushes the code to GitHub**. We have a **GitHub webhook configured with Jenkins**, so whenever there is a code change, GitHub triggers the Jenkins pipeline.

Once the pipeline starts, Jenkins **checks out the latest code** and performs the required build steps.

As part of our **Shift-Left DevSecOps strategy**, we integrate **security and quality checks early in the CI/CD pipeline** rather than waiting until the application is deployed.

First, we perform **code-quality analysis using SonarQube**. SonarQube checks the code for issues such as bugs, code smells, and maintainability problems. We also use **Quality Gates** to ensure the code meets the required quality standards.

Along with code-quality checks, we perform **security and vulnerability scanning early in the pipeline**. This helps us identify security vulnerabilities before the application reaches the deployment stage. If the quality or security checks fail, the pipeline stops and the application is not promoted to the next stage.

Once the **quality and security checks are successful**, Jenkins builds the application into a **Docker image**.

We then **tag the Docker image** and push it to **Amazon ECR**, which is our container image repository.

After the image is successfully pushed to ECR, we deploy the application to **Amazon EKS using Helm charts**. Helm helps us manage Kubernetes resources and application configurations in a consistent and repeatable way.

Once the deployment is completed, **Kubernetes manages the application Pods, Services, and other resources**. Depending on the requirement, we also use deployment strategies such as **blue-green deployments** to reduce downtime and deployment risk.

Finally, we monitor the application and Kubernetes environment using **Prometheus and Grafana**. Prometheus collects metrics, while Grafana provides dashboards for monitoring application and infrastructure health.

So, our overall CI/CD and DevSecOps flow is:

**Developer → GitHub → Jenkins → Build → SonarQube → Quality Gate → Security/Vulnerability Scanning → Docker Build → Amazon ECR → Amazon EKS using Helm → Prometheus & Grafana**

The main idea behind our **Shift-Left DevSecOps approach** is to identify **code-quality and security issues as early as possible in the development and CI/CD lifecycle**. This helps us reduce security risks, avoid expensive fixes later, and deliver applications more securely and reliably.

# Explain Your Role

Sure. In my current project, I’m working as a **DevOps Engineer**, mainly responsible for **CI/CD automation, AWS, Docker, Kubernetes, Terraform, Ansible, DevSecOps, and monitoring**.

My primary responsibility is to **build and maintain Jenkins CI/CD pipelines** and automate the application deployment process from **GitHub to Kubernetes**.

As part of our **Shift-Left DevSecOps strategy**, I integrate **SonarQube, Quality Gates, and security and vulnerability scanning** into the pipeline so that we can identify quality and security issues early.

I also work with **Docker** to build container images and push them to **Amazon ECR**, and I use **Helm to deploy applications to Amazon EKS**.

On the infrastructure side, I use **Terraform** for AWS infrastructure provisioning and **Ansible** for configuration and deployment automation.

I also work with **Kubernetes troubleshooting, scaling, blue-green deployments, and monitoring using Prometheus and Grafana**.

Apart from implementation, I’m involved in **troubleshooting pipeline failures, deployment issues, and infrastructure-related problems**.

So overall, my role is to make the application delivery process **automated, secure, reliable, and scalable**.

# Explain Jenkins Pipeline

Sure. In my project, we use a **Jenkins Declarative Pipeline** to automate the complete CI/CD process from code integration to application deployment.

The pipeline is triggered when a developer pushes code to **GitHub**. We have a **GitHub webhook configured with Jenkins**, which automatically triggers the pipeline.

The pipeline mainly consists of the following stages:

1. **Checkout** – Jenkins checks out the latest source code from the GitHub repository.

2. **Build** – We build the application and generate the required artifacts.

3. **Code Quality** – We run **SonarQube analysis** to identify bugs, code smells, and other code-quality issues.

4. **Quality Gate** – Jenkins checks the SonarQube Quality Gate. If the Quality Gate fails, the pipeline stops and the application is not promoted further.

5. **Security and Vulnerability Scanning** – As part of our **Shift-Left DevSecOps strategy**, we perform security and vulnerability checks early in the pipeline. If critical issues are identified, the pipeline can be stopped.

6. **Docker Build** – If all the required checks pass, Jenkins builds the application into a **Docker image**.

7. **Push to ECR** – We tag the Docker image and push it to **Amazon ECR**.

8. **Deploy to EKS** – Jenkins uses **Helm** to deploy the application to **Amazon EKS**. Helm manages the Kubernetes deployment and application configuration.

9. **Verification** – After deployment, we verify the Kubernetes resources and application health.

10. **Monitoring** – The deployed application and Kubernetes environment are monitored using **Prometheus and Grafana**.

We also have appropriate **post-build actions and notifications** to handle successful or failed pipeline executions.

So, in simple terms, the pipeline flow is:

**GitHub → Jenkins → Checkout → Build → SonarQube → Quality Gate → Security Scan → Docker Build → ECR → Helm → EKS → Verification → Prometheus/Grafana**

The main purpose of the pipeline is to provide **automated, repeatable, secure, and reliable application delivery**.

