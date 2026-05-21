A complete CI/CD pipeline for a web application using GitHub Actions, Docker, Terraform, and AWS ECS Fargate — achieving automated testing, containerisation, and zero-downtime deployments.

🎯 Project Overview
This case study implements DevOps best practices to solve real-world software delivery problems:
ProblemSolutionManual builds & deploymentsAutomated GitHub Actions pipeline"Works on my machine"Docker containerisationConfiguration drift across environmentsTerraform Infrastructure as CodeInconsistent deploymentsRolling updates via AWS ECS FargateLate bug discoveryAutomated unit, integration & E2E tests

🔧 Tech Stack
ToolPurposeGit / GitHubVersion control + distributed branchingGitHub ActionsCI/CD automation engine (.github/workflows/ci-cd.yml)DockerContainerise frontend, backend, and servicesDocker HubVersioned image registryTerraformInfrastructure as Code (AWS provisioning)AWS ECS FargateServerless container deploymentAWS ALBLoad balancing + health checksAWS CloudWatchLogging and monitoringJest / JUnitAutomated testing

🚀 Pipeline Architecture
Developer pushes code
        │
        ▼
┌─────────────────────────────────┐
│   CI STAGE (GitHub Actions)     │
│                                 │
│  1. Code checkout               │
│  2. Static analysis & linting   │
│  3. Unit tests (JUnit / Jest)   │
│  4. Integration tests           │
│  5. Docker image build          │
│  6. Push image → Docker Hub     │
└─────────────┬───────────────────┘
              │ (all gates passed)
              ▼
┌─────────────────────────────────┐
│   CD STAGE – STAGING            │
│                                 │
│  1. Terraform apply (IaC)       │
│  2. ECS Task Definition update  │
│  3. Rolling deployment (Fargate)│
│  4. E2E tests on staging URL    │
│  5. ALB health checks           │
└─────────────┬───────────────────┘
              │ (manual approval gate)
              ▼
┌─────────────────────────────────┐
│   CD STAGE – PRODUCTION         │
│                                 │
│  1. Authorised team approval    │
│  2. Same image → production     │
│  3. Zero-downtime rolling update│
│  4. CloudWatch monitoring       │
└─────────────────────────────────┘

🐳 Docker Setup
The application is split into containerised services:
docker-compose.yml
├── frontend/   → Dockerfile (React / static)
├── backend/    → Dockerfile (Node.js / Java)
└── database/   → PostgreSQL / MySQL
Every Docker image is tagged with a unique commit hash for full traceability and instant rollback capability.

🏗️ Infrastructure as Code (Terraform)
Terraform provisions all AWS resources automatically from GitHub Actions after CI passes:
terraform/
├── ecs_cluster.tf      # ECS cluster + Fargate profiles
├── vpc.tf              # VPC, subnets, route tables
├── alb.tf              # Application Load Balancer
├── cloudwatch.tf       # Monitoring and alarms
└── variables.tf

🧪 Testing Strategy
LayerToolWhenUnit TestsJUnit / JestEvery commitIntegration TestsCustom test suiteEvery commitEnd-to-End TestsE2E frameworkAfter staging deployHealth ChecksAWS ALBAfter every deploy

✅ Results & Benefits

✅ Fully automated pipeline from commit to production
✅ Docker ensures identical environments (Dev → Staging → Production)
✅ Zero-downtime deployments via ECS rolling updates
✅ Manual approval gate protects production from untested code
✅ CloudWatch provides real-time observability


🛠️ How to Run Locally
bash# Clone the repo
git clone https://github.com/NguinfackFranck/DevOps-project.git
cd DevOps-project

# Start all services
docker-compose up --build

# Run tests
docker-compose run backend npm test
