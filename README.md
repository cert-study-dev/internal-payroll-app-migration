# Internal Payroll App Migration

## About the Project

A cloud infrastructure simulation replicating the lift-and-shift migration of a legacy HR Payroll system to AWS. Designed to address the operational challenge of retiring failing on-premise hardware, this project enforces strict network isolation to ensure the mock sensitive dataset remains inaccessible from the public internet.

## Key Technical Components

- **Infrastructure as Code (Terraform)**:
Fully automated provisioning of a multi-tier VPC architecture, ensuring repeatable and consistent deployments.

- **Secure Networking**:
Implementation of a strict Private Subnet architecture with no direct internet gateways for the application layer.

- **Compute & Application (Nginx)**:
Usage of EC2 instances running Nginx to serve the simulated legacy internal application.

- **Persistent Storage (EBS)**:
Dynamic attachment and mounting of EBS Volumes to simulate the migration and persistence of historical payroll data.

- **Security & Compliance**:
A hardened Bastion Host serving as the single secure entry point, featuring automated Audit Logging scripts for compliance tracking.

## Limitations & Sandbox Constraints

Since this project was designed to be deployable within a strict 3-hour ephemeral sandbox environment (KodeKloud), certain architectural choices were made to balance speed with best practices:

- **Local Terraform State**:
A remote backend (S3 + DynamoDB) was intentionally omitted since it's an ephemeral sandbox environment that resets every 3 hours. State is managed locally for the duration of the session.

- **Simplified IAM Roles**:
Due to sandbox permission restrictions on creating new IAM entities, the solution leverages standard AWS Access Keys injected via CI/CD rather than custom Instance Profiles.

- **HTTP vs HTTPS**:
The internal application uses HTTP (Port 80) for simplicity within the private network. In a real-world production scenario, an internal Certificate Authority (CA) would be established to enforce TLS encryption in transit.

- **Single Availability Zone**:
To minimize deployment time and resource quotas within the sandbox, the architecture currently resides in a single AZ (us-east-1a). A production variant would span multiple AZs for high availability.
