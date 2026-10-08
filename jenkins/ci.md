**Recruiter:** Can you explain the CI/CD process in your project?

**Me:** Sure. In our project, everything is automated, and we follow a DevSecOps, Shift-Left approach, which basically means we try to catch quality and security problems as early as possible instead of after deployment.

It starts when a developer commits code to our Git repository. That automatically triggers our Jenkins pipeline. Jenkins checks out the latest code, then Maven builds the application and runs the unit tests. If the build or any test fails, the pipeline stops right there, we fix the issue, and only then move forward.

After that comes the part I like most, the Shift-Left part. Early in the CI stage we run SonarQube analysis, which checks for bugs, vulnerabilities, code smells, and other code-quality issues. We've defined quality gates, so if the code doesn't meet our standards, the pipeline stops. We also run AppScan for security scanning, so vulnerabilities are identified before the application is ever deployed.

Once the build, quality checks, and security scans all pass, we deploy the application to our Dev environment, which runs on Kubernetes. We use Helm to manage the Kubernetes deployment configurations. After the deployment, we run API and functional tests to make sure the application is working as expected.

For continuous deployment, we use Argo CD with a GitOps approach. The desired deployment configuration is maintained in Git, and Argo CD keeps the Kubernetes cluster in sync with it, so Git is our single source of truth. Once the application is tested and approved, we promote it to Production through Argo CD, based on our release and approval process.

And for sensitive information like passwords and tokens, we never hardcode them. We manage them securely using Jenkins Credentials.

So overall, by catching code-quality and security issues early, we reduce security risks, avoid late-stage defects, and get more reliable and consistent deployments.

**Recruiter:** What happens if the quality gate fails?

**Me:** The pipeline stops immediately and the build doesn't move forward. The developer gets the SonarQube report, fixes the issues, such as bugs, vulnerabilities, or code smells, commits again, and the pipeline re-runs. That way, poor-quality code never reaches the Dev environment.

**Recruiter:** Why do you use Argo CD and GitOps?

**Me:** Because Git becomes the single source of truth for what should be running in the cluster. Every change is tracked and reviewable, deployments are consistent, and if something goes wrong we can roll back to a previous version in Git. Argo CD continuously compares the cluster with Git and keeps them in sync.

**Recruiter:** Why Helm?

**Me:** Helm lets us package and manage our Kubernetes configurations as templates. We can reuse the same chart across environments and just change the values, like image version or replicas, instead of maintaining separate YAML files for each environment.
**Recruiter:** Can you explain your project architecture?

**Me:** Sure. I'll explain it in two parts: the **application side** and the **delivery side**.

On the application side, we have a **Java application built using Maven**. We package the application as a container image and run it on Kubernetes. We have separate deployments for different environments like Dev and Production, and we use **Helm to manage the Kubernetes deployment configuration**. This allows us to use the same Helm chart across environments with different values.

The application exposes APIs, which we use for API and functional testing after deployment.

On the delivery side, most of my work was around the **CI/CD pipeline**. A developer pushes code to Git, which triggers Jenkins. Jenkins checks out the code, Maven builds the application and runs the unit tests. Then SonarQube performs code-quality analysis and checks the defined quality gates. We also run AppScan for security scanning.

If all the checks pass, the application is deployed to the Dev environment on Kubernetes using Helm. After deployment, we run API and functional tests to verify that the application is working as expected.

For continuous deployment, we use **Argo CD with a GitOps approach**. The desired Kubernetes configuration is maintained in Git, and Argo CD continuously synchronizes the cluster with that desired state. After testing and approval, we promote the application to Production through Argo CD.

For sensitive information like passwords and tokens, we don't hardcode them. We manage them securely using **Jenkins Credentials**.

So, at a high level, the flow is:

**Git → Jenkins → Maven → Unit Tests → SonarQube → AppScan → Helm → Kubernetes Dev → API/Functional Tests → Argo CD → Production**

Overall, our architecture follows a **DevSecOps and Shift-Left approach**, where we identify quality and security issues early in the CI pipeline, while Git serves as the source of truth for the Kubernetes deployment state.

                  Developer
                      |
                      ↓
                    Git
                      |
                      ↓
                   Jenkins
                      |
             ┌────────┴────────┐
             ↓                 ↓
           Maven           Unit Tests
             |
             ↓
         SonarQube
       Quality Gate
             |
             ↓
          AppScan
      Security Scan
             |
             ↓
           Helm
             |
             ↓
       Kubernetes - Dev
             |
             ↓
     API / Functional Tests
             |
             ↓
          Argo CD
       GitOps / Sync
             |
             ↓
      Kubernetes - Prod
