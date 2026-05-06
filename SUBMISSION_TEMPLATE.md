<div align="center">

# PA4 Submission: TaskFlow Pipeline

<img alt="GitHub only" src="https://img.shields.io/badge/Submit-GitHub%20URL%20Only-10b981?style=for-the-badge">
<img alt="Total points" src="https://img.shields.io/badge/Total-100%20points-7c3aed?style=for-the-badge">

</div>

<div style="background:#f5f3ff;color:#111827;border-left:6px solid #6330bc;padding:14px 18px;border-radius:10px;margin:18px 0;">
Copy this file to <code style="color:#111827;background:#ddd6fe;padding:2px 4px;border-radius:4px;">SUBMISSION.md</code>. Put every screenshot in <code style="color:#111827;background:#ddd6fe;padding:2px 4px;border-radius:4px;">docs/</code>, embed it under the correct task, and write a short description below each image explaining what it proves. The grader should not need any file outside this repository.
</div>

## Student Information

| Field | Value |
|---|---|
| Name | Abdullah Tahir |
| Roll Number | 27100219 |
| GitHub Repository URL | https://github.com/0-0-8/CS487-PA4 |
| Resource Group | `rg-sp26-27100219` |
| Assigned Region | `ukwest` |

## Evidence Rules

- Use relative image paths, for example: `![AKS nodes](docs/aks-nodes.png)`.
- Every image must have a 1-3 sentence description below it.
- Azure Portal screenshots must show the resource name and enough page context to identify the service.
- CLI screenshots must show the command and output.
- Mask secrets such as function keys, ACR passwords, and storage connection strings.

## Task 1: App Service Web App (15 points)

### Evidence 1.1: Forked Repository

![Forked Repository](docs/ss/1.1.png)

This screenshot shows my forked GitHub repository for the PA4 assignment. The repository contains the starter project structure including the frontend, Durable Functions app, validator API, and report job services.



### Evidence 1.2: App Service Overview

![App Service Overview](docs/ss/1.2.png)

This screenshot shows the Azure App Service overview page for `webapp-27100219`. The web app is deployed in the `rg-sp26-27100219` resource group in the `UK West` region and is publicly accessible through the shown URL.



### Evidence 1.3: Deployment Center / GitHub Actions

![Deployment Center](docs/ss/1.3.png)

This screenshot shows the deployment configuration connected to my GitHub fork. GitHub Actions was used to automatically deploy the frontend application to Azure App Service after pushes to the repository.



### Evidence 1.4: Live Web UI

![Live Web UI](docs/ss/1.4.png)

This screenshot shows the TaskFlow frontend successfully running in the browser. The App Service is correctly serving the frontend application publicly.



### Evidence 1.5: Environment Variables

![Environment Variables](docs/ss/1.5.png)

This screenshot shows the environment variables being correctly set in the webapp settings.

---

## Task 2: Azure Container Registry (15 points)

### Evidence 2.1: ACR Overview

![ACR Overview](docs/ss/2.1.png)

This screenshot shows the Azure Container Registry used in the project. The registry is deployed in the `rg-sp26-27100219` resource group and stores all container images used by AKS, ACI, and Azure Functions.


### Evidence 2.2: Docker Builds

![Docker Builds](docs/ss/2.2.png)

This screenshot shows successful Docker image builds for `validate-api`, `report-job`, and `func-app`. Each image was built locally from its respective project folder before being pushed to ACR.


### Evidence 2.3: ACR Repositories

![ACR Repositories](docs/ss/2.4.png)
![ACR Repositories](docs/ss/2.5.png)

This screenshot confirms that the `validate-api:v1`, `report-job:v1`, and `func-app:v1` images were successfully pushed to Azure Container Registry.

### Evidence 2.4: Post Curl

![Post Curl](docs/ss/2.3.png)

This screenshot confirms that the post curl returns the valid output.

---

## Task 3: Durable Function Implementation (12 points)

### Evidence 3.1: Completed Function Code

[function_app.py](function-app/function_app.py)

The Durable Function orchestrator first calls the validator API and then triggers the report-generation workflow if validation succeeds. The orchestration manages workflow sequencing and status tracking.


### Evidence 3.2: Local Function Handler Listing

![Function Handler Listing](docs/ss/3.1.png)

This screenshot shows the Durable Functions runtime discovering the HTTP starter, orchestrator, and activity functions during local execution using `func start`.

---

## Task 4: Function App Container Deployment (8 points)

### Evidence 4.1: Function App Container Configuration

![Function App Container](docs/ss/4.1.png)

This screenshot shows that the Azure Function App is configured to use the `func-app:v1` image stored in Azure Container Registry.


### Evidence 4.2: Orchestration Smoke Test

![Smoke Test](docs/ss/4.3.png)

This screenshot shows a successful orchestration start request using `curl`. The returned orchestration ID and status URLs confirm that the Durable Functions runtime accepted the workflow request.


### Evidence 4.3: Expected Failed Status Before Downstream Wiring

![Expected Failure](docs/ss/4.4.png)

This screenshot shows the expected orchestration failure before `VALIDATE_URL` was configured. The workflow could not contact the validator service at this stage.


### Evidence 4.4:  Functions list in the Portal 

![Expected Failure](docs/ss/4.2.png)

This screenshot shows the Functions list in the Portal showing http_starter, my_orchestrator, validate_activity, report_activity


---

## Task 5: AKS Validator (15 points)

### Evidence 5.1: AKS Cluster

![AKS Cluster](docs/ss/5.x.png)

This screenshot shows the AKS cluster successfully deployed in the `UK West` region under the `rg-sp26-27100219` resource group with one running node.


### Evidence 5.2: Kubernetes Nodes and Pods

![Kubernetes Nodes](docs/ss/5.1.png)
![Kubernetes Pods](docs/ss/5.2.png)

This screenshot shows the Kubernetes node and validator pod running successfully. The validator service is scheduled correctly inside the AKS cluster.


### Evidence 5.3: Kubernetes Service

![Kubernetes Service](docs/ss/5.3.png)

This screenshot shows the `validate-service` LoadBalancer service with an external public IP and exposed port used by the Durable Function validator step.


### Evidence 5.4: Validator API Tests

![Validator Tests](docs/ss/5.4a.png)

![Validator Tests](docs/ss/5.4b.png)

These screenshots show successful `/health` and `/validate` API tests. Orders with quantity greater than 100 are rejected according to the validator rules.


### Evidence 5.5: Function App VALIDATE_URL

![VALIDATE_URL](docs/ss/5.5.png)

This screenshot shows the `VALIDATE_URL` application setting configured in the Function App. The Durable Function uses this endpoint to communicate with the AKS validator service.


### Evidence 5.6: AKS Idle Behavior

![AKS Idle Behavior](docs/ss/5.5.png)

This screenshot demonstrates that the AKS node remains active even when no requests are being processed. Kubernetes infrastructure continues running during idle periods.

---

## Task 6: ACI Report Job (15 points)

### Evidence 6.1: Blob Container

![Blob Container](docs/ss/6.1.png)

This screenshot shows the `reports` blob container where generated PDF reports are stored after successful orchestration completion.


### Evidence 6.2: Manual ACI Run

![ACI Run](docs/ss/6.2.png)

This screenshot shows the manual ACI execution for the report-generation job. The container exits successfully after generating and uploading the PDF report.


### Evidence 6.3: ACI Logs

![ACI Logs](docs/ss/6.3.png)

This screenshot shows the logs generated by the report job container. The logs confirm successful PDF creation and upload to Azure Blob Storage.


### Evidence 6.4: Generated PDF

![Generated PDF](docs/ss/6.4.png)

This screenshot shows the generated PDF report stored inside the `reports` blob container. This confirms that the ACI job successfully uploaded the file.


### Evidence 6.5: Function App Managed Identity and IAM

![Managed Identity](docs/ss/6.5.png)

![IAM Roles](docs/ss/6.6.png)

These screenshots show the Function App managed identity and its assigned IAM permissions. The Function App requires these permissions to create and manage ACI containers dynamically.


### Evidence 6.6: Report App Settings

![Report App Settings](docs/ss/6.6.png)

This screenshot shows the `REPORT_*`, `ACR_*`, and subscription-related environment variables configured in the Function App. These settings are used for ACI deployment and report storage access.

---

## Task 7: End-to-End Pipeline (15 points)

### Evidence 7.1: Web App Wiring

![Web App Wiring](docs/ss/7.1.1.png)

![Web App Wiring](docs/ss/7.1.2.png)

![Web App Wiring](docs/ss/7.1.3.png)

These screenshots show the frontend application configured with the Durable Function start and status endpoints. The frontend uses these URLs to start workflows and poll orchestration progress.


### Evidence 7.2: Happy Path UI

![Happy Path](docs/ss/7.1.1.png)

![Happy Path](docs/ss/7.1.2.png)

![Happy Path](docs/ss/7.1.3.png)

These screenshots show a successful end-to-end workflow execution from order submission to completed report generation with a downloadable PDF URL.


### Evidence 7.3: Backend Participation

![Backend Evidence](docs/ss/7.1.6.png)

![Backend Evidence](docs/ss/7.1.7.png)

![Backend Evidence](docs/ss/7.1.8.png)

![Backend Evidence](docs/ss/7.1.4.png)

![Backend Evidence](docs/ss/7.1.5-a.png)
![Backend Evidence](docs/ss/7.1.5-b.png)
![Backend Evidence](docs/ss/7.1.5-c.png)


These screenshots trace the same order ID through Durable Functions, AKS validation, ACI report generation, and Blob Storage upload.
PDF in viewer after downlaoding it from the blob storage manually.


### Evidence 7.4: Reject Path UI

![Reject Path](docs/ss/7.2.1-a.png)
![Reject Path](docs/ss/7.2.1-b.png)
![Reject Path](docs/ss/7.2.2.png)
![Reject Path](docs/ss/7.2.3.png)

This screenshot shows an invalid order with quantity greater than 100 being rejected by the validator service. Since validation failed, no ACI report container was created.


---

## Task 8: Write-up and Architecture Diagram (5 points)

### Evidence 8.1: Architecture Diagram

![as](docs/ss/arch.png)

This diagram shows the overall TaskFlow architecture including GitHub, App Service, Durable Functions, AKS, ACI, Azure Blob Storage, Azure Container Registry, and IAM integration.

---

### Question 8.2: Service Selection

App Service is used for the frontend because it provides managed hosting for the web app with easy deployment, scaling, and HTTPS support. It simplifies hosting without requiring VM management.

Durable Functions are used for workflow orchestration because the project requires multiple sequential steps like validation and report generation. They provide retries, state management, and monitoring automatically.

AKS is used for the validation API because it is a continuously running containerized microservice. Kubernetes provides container orchestration, scaling, and networking support.

ACI is used for PDF report generation because it is a temporary batch workload. ACI is cheaper and simpler for short-lived containers since it runs only when needed.

---

### Question 8.3: ACI vs AKS

AKS keeps cluster nodes running even when idle, while ACI containers run only during execution and stop afterward.

AKS has higher constant cost because VMs stay active continuously. ACI follows a pay-per-use model and is cheaper for temporary workloads.

AKS requires Kubernetes management and configuration. ACI is simpler because containers run directly without cluster management.

---

### Question 8.4: Durable Functions vs Plain HTTP

Durable Functions manage workflow state and guarantee that validation finishes before report generation starts.

They also provide retries, long-running workflow support, and monitoring. Implementing this manually with plain HTTP would require custom polling and error-handling logic.

---

### Question 8.5: Cost Review

Cost Management access was not granted for the subscription/resource group, so cost screenshots could not be retrieved.

The most expensive resource is likely AKS because the Kubernetes node runs continuously even during idle periods.

---

### Question 8.6: Challenges Faced

One issue was that the Function App failed due to storage authorization errors. This was fixed by configuring Managed Identity authentication and assigning proper storage roles.

Another issue involved storage access returning 403 errors. Debugging included checking IAM roles, storage permissions, and network restrictions before resolving the configuration problems.