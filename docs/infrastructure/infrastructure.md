# Infrastructure as Code

Infrastructure as Code for deploying the Campus Device Loan System on DigitalOcean App Platform.

## Overview

The infrastructure includes:
- **Terraform** - Infrastructure provisioning and management
- **DigitalOcean App Platform** - Platform-as-a-Service deployment
- **GitHub Actions** - CI/CD pipeline automation
- **Terraform Cloud** - Remote state management

## Architecture

The system is deployed on DigitalOcean App Platform with:
- **Device Service** - Backend microservice (Node.js)
- **Loan Service** - Backend microservice (Node.js)
- **Frontend** - Next.js web application
- **PostgreSQL Database** - Managed database cluster

## Terraform Configuration

Terraform is used to provision and manage the DigitalOcean App Platform resources.

### Structure

```
infra/terraform/
├── app.tf          # App Platform application definitions
├── database.tf     # Database cluster configuration
├── provider.tf     # DigitalOcean provider configuration
├── project.tf      # DigitalOcean project configuration
├── variables.tf    # Variable definitions
├── outputs.tf      # Output values
└── env/
    └── dev.tfvars  # Environment-specific variables
```

### Resources Managed

- DigitalOcean App Platform applications (device-service, loan-service, frontend)
- PostgreSQL database cluster
- App Platform project
- Environment variables and secrets
- Health checks and scaling configurations

### Remote State

Terraform uses Terraform Cloud for remote state management:
- State is stored remotely in Terraform Cloud
- Prevents state file conflicts in team environments
- Enables state locking for concurrent operations

## CI/CD Pipeline

GitHub Actions workflow (`.github/workflows/deploy-full-pipeline.yml`) automates:

### Pipeline Stages

1. **Terraform Plan/Apply**
   - Provisions/updates DigitalOcean App Platform resources
   - Manages database cluster
   - Configures environment variables

2. **Database Migrations**
   - Runs database migrations from `/database` folder
   - Executes before service deployments

3. **Service Deployments**
   - Device Service deployment
   - Loan Service deployment
   - Frontend deployment

### Trigger Events

- Push to `main` branch (automatic deployment)
- Manual workflow dispatch

### Environment Variables

The pipeline uses GitHub Actions secrets and variables:
- `TF_TOKEN_APP_TERRAFORM_IO` - Terraform Cloud authentication
- `TFC_ORGANIZATION` - Terraform Cloud organization
- `TFC_WORKSPACE` - Terraform Cloud workspace
- DigitalOcean API tokens (managed via App Platform)

## Deployment Process

1. **Infrastructure Provisioning**
   ```bash
   # Terraform automatically runs in CI/CD pipeline
   terraform init
   terraform plan
   terraform apply
   ```

2. **Database Setup**
   - Migrations run automatically via GitHub Actions
   - Uses connection string from Terraform outputs

3. **Application Deployment**
   - Services are automatically deployed to App Platform
   - Build and deploy commands configured in Terraform
   - Health checks ensure successful deployment

## Environment Configuration

### Development Environment

- Configured via `infra/terraform/env/dev.tfvars`
- Separate App Platform apps for each service
- Shared PostgreSQL database cluster

### Environment Variables

Managed via Terraform and App Platform:
- `DATABASE_URL` - PostgreSQL connection string
- `JWT_SECRET` - Shared secret for JWT tokens
- `JWT_EXPIRES_IN` - Token expiration time
- `PORT` - Service port
- `NODE_ENV` - Environment (production)
- `CORS_ORIGIN` - Allowed CORS origins
- `LOG_LEVEL` - Logging level

## Monitoring & Health Checks

- **Health Endpoints**: `/health`, `/ready`, `/metrics` on all services
- **App Platform Monitoring**: Built-in health checks and auto-scaling
- **Database Monitoring**: Managed via DigitalOcean dashboard

## Scaling

App Platform automatically handles:
- Horizontal scaling based on traffic
- Vertical scaling based on resource usage
- Database connection pooling

## Related Documentation

- [Main Project README](../../README.md) - Project overview
- [Device Service](../backend/device-service.md) - Device service documentation
- [Loan Service](../backend/loan-service.md) - Loan service documentation
- [Database Guide](../database/database.md) - Database migrations and setup

