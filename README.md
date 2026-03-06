[![Serverless Container Framework](https://github.com/Unity-Billal-mesloub/)](https://serverless.com/containers/docs)

**One solution to deploy serverless workloads everywhere** - Serverless Container Framework (SCF) is a unified development and deployment experience for containers on serverless platforms.

In this initial release, SCF focuses on delivering an API architecture that leverages AWS Application Load Balancer for request routing, allowing developers to freely mix and transition between AWS Lambda and AWS ECS Fargate compute options, accompanied by a rich development experience.

This repo contains examples and other educational content on SCF. The code for SCF ships within the [Serverless Framework](https://github.com/Unity-Billal-mesloub/serverless).

* [Overview Video (90 seconds)](https://youtu.be/KXNYemGzda4)
* [Feedback Form](https://form.typeform.com/to/iqaERaLP)
* [Documentation](https://serverless.com/containers/docs)

# Features

### Unified Container Development & Deployment
- Deploy seamlessly to AWS Lambda and ECS Fargate via a single workflow (and more providers soon)
- Mix Lambda and Fargate compute within a single API
- Switch compute platforms instantly without code rewrites or downtime
- Optimize container builds automatically for each compute service
- Write Node.js and Python apps naturally - No Dockerfiles needed
- Get production-ready infrastructure in seconds with automated VPC, networking & ALB setup

### Rich Development Experience
- Develop Lambda and Fargate containers rapidly with true local emulation
- Route and simulate AWS ALB requests via localhost
- Accelerate development with instant hot reloading
- Inject live AWS IAM roles into your containers
- Enjoy an elegant logging and debugging experience

### Production-Ready Features
- Smart code/config change detection for deployments
- Supports one or multiple custom domains on the same API
- Automatic SSL certificate management
- Secure AWS IAM and network defaults
- Load environment variables from .env, AWS Secrets Manager, AWS Systems Manager Parameter Store, HashiCorp Vault, HashiCorp Terraform state, and more via [Serverless Framework Variables](https://www.serverless.com/framework/docs/guides/variables)
- Multi-cloud support coming soon

# Configuration

Serverless Container Framework offers simple YAML to deliver complex architectures via a `serverless.containers.yml` file. Here is a simple example of a full-stack application.

To see the full list of configuration options, see the [Configuration Docs](https://serverless.com/containers/docs/configuration).

```yaml
namespace: acmeinc

deployment:
  type: awsApi@1.0

containers:
  # Web (Frontend)
  service-web:
    src: ./web
    routing:
      domain: acmeinc.com
      pathPattern: /*
    compute:
      type: awsLambda
  # API (Backend)
  service-api:
    src: ./api
    routing:
      domain: api.acmeinc.com
      pathPattern: /api/*
      pathHealthCheck: /health
    compute:
      type: awsFargateEcs
      awsFargateEcs:
        memory: 4096
        cpu: 1024
      environment:
        HELLO: world
      awsIam:
        customPolicy:
          Version: "2012-10-17"
          Statement:
            - Effect: Allow
              Action:
                - dynamodb:GetItem
              Resource:
                - "*"
```


# Quick Start

### Prerequisites
- Node.js 20.x or later
- AWS Account with administrative access
- Docker installed and running

### Installation & Setup

1. Install the Serverless Framework CLI globally:

```bash
npm install -g serverless
```

2. Configure your AWS credentials using one of these methods:

```bash
# Option 1: AWS CLI (recommended)
aws configure

# Option 2: Environment variables
export AWS_ACCESS_KEY_ID=your-key-id
export AWS_SECRET_ACCESS_KEY=your-access-key
export AWS_SESSION_TOKEN=your-session-token
```

### Getting Started

1. Start with an example project by cloning the repository:
```bash
git clone https://github.com/serverless/containers.git
```

2. Navigate to the example project directory, and install any dependencies:
```bash
cd example-express/service
npm install
```

### Development

Ensure you are within the directory containing the `serverless.containers.yml` file.
```bash
cd example-express
```

Start the local development environment:
```bash
serverless dev
```

This starts a local emulation of AWS Application Load Balancer at `http://localhost:3000`. This will forward requests to your containers. Logs, requests and more from your containers will be available in the terminal. Your containers will auto-reload or rebuild on code changes.

### Deployment

Deploy to AWS:
```bash
serverless deploy
```

The initial deployment creates AWS resources (ALB, VPC, etc.) and takes ~5-10 minutes. Subsequent deploys are faster.

### Cleanup

Remove your deployment:
```bash
# Remove application only
serverless remove

# Remove all AWS resources including VPC
serverless remove --force
```

### Troubleshooting
- Ensure Docker daemon is running for local development
- Check AWS credentials are properly configured using `aws sts get-caller-identity`
- View detailed logs with `serverless dev --debug` or `serverless deploy --debug`

### Documentation

Check out the [Documentation](https://serverless.com/containers/docs) for information on getting started, development, deployment, and more.

## Serverless Container Framework FAQ
### How does SCF differ from Serverless Framework?
Here's the concise FAQ in markdown format:

## Serverless Container Framework FAQ

### How does SCF differ from Serverless Framework?

Serverless Framework specializes in AWS Lambda deployments, while Serverless Container Framework (SCF) focuses on container-based deployments. Key SCF advantages:

- Multi-cloud deployment without provider-specific expertise
- Unified development and deployment workflow
- Container portability without code changes
- Integration with existing container tools

### Why was SCF created?

SCF addresses specific needs:
- Migration path for high-volume AWS Lambda functions
- Better enterprise integration through container standards
- Leveraging improved container support in AWS Lambda
- Bridging the gap between serverless and traditional container workflows

### What are the pricing details?

- Free for developers and organizations with <$2M annual revenue
- For larger organizations: 1 credit per deployed container/month
- Containers deployed <10 days/month incur no charges
- No charges for:
  - Individual users
  - Usage (requests/compute)
  - CLI actions
- Check out our [pricing page](https://www.serverless.com/pricing) for more details.

### How does SCF compare to other container tools?

While tools like Terraform and Pulumi offer container deployment capabilities, SCF provides a more comprehensive developer experience:
- Local cloud environment emulation
- ALB-compatible local API
- Real-time log streaming
- Hot module reloading
- Zero-downtime switching between Lambda and ECS Fargate

### What are the limitations?

**Compared to AWS Lambda:**
- Longer deployment times due to:
  - Larger container images
  - Additional AWS Fargate infrastructure setup
- 1MB payload limit for requests/responses (including headers) when using ALB

**Compared to other container tools:**
- Newer platform still evolving, though offering superior developer experience









