In my project, we use GitHub for source code management and Jenkins for CI/CD automation. We have separate CI and CD pipelines.

In the CI pipeline, when developers push code, Jenkins reads the application version from package.json, installs the dependencies and performs the required testing and security validations. We also check GitHub Dependabot alerts and fail the pipeline if there are open HIGH or CRITICAL vulnerabilities.

After that, Jenkins builds the Docker image using a multi-stage Dockerfile. We tag the image using the application version and push it to Amazon ECR. Once the image is pushed, we validate the ECR vulnerability scan results and fail the pipeline if HIGH or CRITICAL vulnerabilities are found.

If all validations pass, the CI pipeline triggers our separate CD pipeline and passes the application version and target environment as parameters.

In the CD pipeline, Jenkins connects to the appropriate Amazon EKS cluster using AWS CLI and kubectl. The deployment configuration is updated with the required image version. We use Helm-based Kubernetes configuration and Argo CD for GitOps synchronization. Argo CD continuously compares the desired state in Git with the state in the EKS cluster and performs synchronization.

For different environments, we have Dev, QA and Production deployment stages. Dev is followed by functional testing, QA is used for integration testing, and Production follows the required change-management and approval process.

After deployment, we validate the Kubernetes rollout status. If the deployment fails, we perform a Helm rollback and verify whether the previous version has been successfully restored.

                     SHIFT-LEFT SECURITY
                            │
Developer ──> GitHub ──> Jenkins CI
                            │
                            ├── Install Dependencies
                            │
                            ├── Unit/Automated Tests
                            │
                            ├── SonarQube
                            │     ├── Bugs
                            │     ├── Vulnerabilities
                            │     ├── Code Smells
                            │     ├── Duplication
                            │     └── Quality Gate
                            │
                            ├── Dependabot
                            │     └── HIGH/CRITICAL check
                            │
                            ▼
                      Docker Build
                     Multi-stage Build
                            │
                            ▼
                         Amazon ECR
                            │
                            ├── Image Vulnerability Scan
                            │     └── HIGH/CRITICAL check
                            │
                            ▼
                    CI Pipeline PASSED
                            │
                            ▼
                     Trigger CD Pipeline
                  Version + Environment
                            │
                            ▼
                       Helm / GitOps
                            │
                            ▼
                         Argo CD
                            │
                            ▼
                          EKS
                            │
             ┌──────────────┼──────────────┐
             ▼              ▼              ▼
            DEV             QA        PRODUCTION
             │              │              │
        Functional      Integration    Approval /
          Testing          Testing     Change Mgmt
             │              │              │
             └──────────────┴──────────────┘
                            │
                            ▼
                    Rollout Validation
                            │
                    ┌───────┴───────┐
                    │               │
                 SUCCESS          FAILURE
                    │               │
                 Continue      Helm Rollback
                                    │
                                    ▼
                            Verify Previous
                               Version

                               