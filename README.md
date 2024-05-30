Project Description
This project implements a full-stack DevOps pipeline for the automated deployment of a Java-based application using Jenkins, Docker, and Kubernetes on AWS EKS. The project integrates various tools to achieve seamless continuous integration and continuous deployment (CI/CD), showcasing proficiency in DevOps practices and workflow automation.

Key Responsibilities

Source Code Management
Git: Utilized for version control and source code management, ensuring efficient collaboration and tracking of code changes.

Build and Compilation
Maven: Employed for compiling Java dependencies and building the application. Managed dependency resolution and build lifecycle effectively.

Unit Testing
JUnit: Implemented comprehensive unit tests to ensure code quality and functionality, enhancing the robustness of the application before deployment.

Code Quality Analysis
SonarQube: Integrated into the Jenkins pipeline for continuous code quality inspection. Ensured adherence to coding standards and identified potential vulnerabilities early in the development process.

Artifact Management
Nexus Repository: Configured for storing build artifacts. Managed versioned artifacts systematically and facilitated efficient artifact retrieval during deployment.

Containerization
Docker: Utilized for containerizing the application, creating a portable and consistent environment for deployment. Built Docker images and pushed them to AWS Elastic Container Registry (ECR).

Continuous Deployment
Helm: Automated the deployment process to Kubernetes EKS Cluster using Helm charts. Managed Kubernetes resources effectively, ensuring smooth and reliable application releases.

Notifications and Monitoring
Slack: Integrated notifications within the Jenkins pipeline to provide real-time updates on the deployment status. Monitored application start, stop, and success events to maintain transparency and quick incident response.

Webhooks
Webhooks: Configured to trigger Jenkins pipeline executions automatically upon code commits, ensuring immediate feedback and continuous integration.

Tools and Technologies Used: 
        Jenkins: Orchestrated the CI/CD pipeline for automated build, test, and deployment processes.
        Git: Version control system for tracking changes and collaborating on code.
        Maven: Build automation tool for managing Java projects and dependencies.
        SonarQube: Platform for continuous inspection of code quality.
        Nexus Repository: Artifact repository for managing build outputs.
        Docker: Containerization platform for creating and managing containers.
        AWS ECR: Managed Docker container registry service.
        Kubernetes EKS: Managed Kubernetes service for deploying and scaling containerized applications.
        Helm: Package manager for Kubernetes applications.
        Slack: Communication platform for deployment notifications.
        Webhooks: Automated triggers for Jenkins pipeline executions.
        Achievements
        Automated CI/CD Pipeline: Implemented a fully automated CI/CD pipeline, reducing deployment time and minimizing manual intervention.
        Enhanced Code Quality: Improved code quality and reliability through integrated testing and code analysis tools.
        Seamless Deployment: Achieved seamless deployment and scaling of applications in a Kubernetes environment.
