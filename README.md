-- CI/CD Pipeline for Node.js Application using GitHub Actions and AWS

-- This README provides an overview of the CI/CD pipeline designed for a Node.js application. The pipeline automates the process of running tests, building and pushing Docker images to Amazon ECR, deploying to an Amazon EKS cluster, and notifying about deployment success or failure.

-- Pipeline Overview

 Key Features:
1. Automatic Testing: Runs tests automatically on pull requests and pushes to the `main` branch.
2. Docker Image Build and Push: Builds a Docker image of the application and pushes it to an Amazon Elastic Container Registry (ECR).
3. Deployment to Kubernetes: Deploys the Docker image to an Amazon Elastic Kubernetes Service (EKS) cluster.
4. Notifications: Sends notifications for deployment success or failure.

-- Workflow Triggers:
- Pull Requests: The pipeline is triggered when a pull request is created or updated for the `main` branch.
- Push Events: The pipeline also runs on code pushes to the `main` branch.

--------

--- Pipeline Jobs and Steps

1. Test
- Purpose: Ensures application passes all tests before proceeding to build and deploy.
- Steps:
  1. Checkout the code from repository.
  2. Set up the Node.js environment (version 16).
  3. Install dependencies using `npm install`.
  4. Run tests using `npm test`.

2. Build & Push Docker Image
- Purpose: Builds the Docker image and pushes it to Amazon ECR.
- Steps:
  1. Checkout the code.
  2. Log in to Amazon ECR using GitHub Actions' AWS credentials.
  3. Build the Docker image & tag it with both `latest` & the GitHub commit SHA.
  4. Push the Docker image to Amazon ECR.

3. Deploy to Kubernetes
- Purpose: Updates the Kubernetes deployment with the new Docker image.
- Steps:
  1. Configure AWS credentials for accessing EKS.
  2. Use `kubectl` to update the Kubernetes deployment with the newly pushed Docker image.

4. Notify
- Purpose: Provides feedback on the deployment outcome.
- Steps:
  1. Send a success notification if the deployment succeeds.
  2. Send a failure notification if the deployment fails.



--- Environment Variables

         Name      ----------       Description                                       
      
 `AWS_REGION`       -- AWS region where resources are deployed.      
 `ECR_REPO_NAME`    --  Name of ECR repository for the Docker image.  
 `CLUSTER_NAME`     -- Name of Amazon EKS cluster.                   
 `DEPLOYMENT_NAME`  -- Kubernetes deployment name.                   
 `CONTAINER_PORT`   -- Port exposed by container.                    

---------

--- Secrets Configuration
The following secrets must be configured in your GitHub repository for the pipeline to work:

         Name         ------        Description 

 `AWS_ACCESS_KEY_ID`     -- AWS access key ID for authentication.         
 `AWS_SECRET_ACCESS_KEY` -- AWS secret access key for authentication.     
 `AWS_ACCOUNT_ID`        -- AWS account ID used for ECR image repository. 

----------

--- Deployment Process
1. When a developer creates or updates a pull request, the Test job runs automatically.
2. If all tests pass, the Build and Push Docker Image job starts.
3. The Docker image is pushed to Amazon ECR.
4. The Deploy to Kubernetes job updates the EKS deployment with the new Docker image.
5. The Notify job sends a message about the success or failure of the deployment.

---------

--- Prerequisites
- An Amazon EKS cluster is already set up.
- An Amazon ECR repository exists for the application.
- The Kubernetes deployment and service manifests are configured and applied to the EKS cluster.
- GitHub Actions has access to the required AWS credentials.

--------

---Notes
- The Docker image is tagged with both `latest` and the specific GitHub commit SHA for version control.
- Notifications can be extended to integrate with tools like Slack or email for better visibility.

--------

--- Future Enhancements
- Add support for staging and production environments.
- Include static code analysis and security checks in the pipeline.
- Enable automatic rollback on deployment failure.

--------

This pipeline ensures automated CI/CD process, making deployments faster, safer, and more reliable.

