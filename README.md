# Student-Teacher Portal: Three-Tier App on AWS ECS

A simple three-tier web application (React frontend, Node.js API, MySQL database), containerized with Docker and deployed to AWS ECS Fargate through GitHub Actions.

Infrastructure (Terraform): **[marbrd/terraform-ecs-3-tiers](https://github.com/marbrd/terraform-ecs-3-tiers)**

> The application is based on [Student-Teacher-Portal](https://github.com/AmanPathak-DevOps/Student-Teacher-Portal-Three-Tier-Application) by Aman Pathak (MIT license). I adapted it for this deployment: the backend reads its database settings from environment variables, nginx proxies `/api` to the backend, and the GitHub Actions workflows build, push and deploy the images using OIDC.

## Run locally

```bash
docker compose up --build
```

Open http://127.0.0.1:8080, and stop with:

```bash
docker compose down -v
```

## Deploy to AWS

1. Create the infrastructure with the [Terraform repository](https://github.com/marbrd/terraform-ecs-3-tiers).
2. In this repository, set the GitHub Actions secret `AWS_ROLE_ARN` (from `terraform output github_actions_role_arn`).
3. Run the **backend** workflow, then the **frontend** workflow.

After that, every push to `main` that changes `backend/` or `frontend/` rebuilds the image, pushes it to ECR and redeploys the ECS service.
