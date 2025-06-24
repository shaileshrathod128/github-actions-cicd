# 🚀 CI/CD Pipeline for Node.js Deployment to EC2 (Multi-Environment)

This repository contains a GitHub Actions workflow that automates CI/CD for a Node.js application with support for multiple environments (`dev`, `staging`, `prod`). It runs tests, builds the app, deploys it to an EC2 instance, and sends a Slack notification based on the deployment outcome.

## 🔧 Setup Instructions

### Prerequisites

- Node.js app with `npm run build` and `npm test`
- EC2 instances with Node.js, Git, PM2 installed, and the repo cloned to a target path
- Slack webhook for notifications

### GitHub Secrets

Add the following secrets to your GitHub repo under **Settings > Secrets and variables > Actions**:

#### Shared

- `SLACK_WEBHOOK_URL` — Slack Incoming Webhook URL

#### Per Environment (`dev`, `staging`, `prod`)

- `DEV_EC2_HOST`, `STAGING_EC2_HOST`, `PROD_EC2_HOST` — EC2 public IP or DNS
- `DEV_EC2_USER`, `STAGING_EC2_USER`, `PROD_EC2_USER` — SSH user (e.g., `ubuntu`)
- `DEV_EC2_KEY`, `STAGING_EC2_KEY`, `PROD_EC2_KEY` — Private key content (`.pem`)
- `DEV_APP_PATH`, `STAGING_APP_PATH`, `PROD_APP_PATH` — App path on EC2

### Usage

1. Navigate to the **Actions** tab in your GitHub repository
2. Select the `CI/CD to EC2 with Multi-Env` workflow
3. Click **Run workflow**
4. Choose the environment (`dev`, `staging`, or `prod`)
5. Click **Run**

The workflow will:

- Install dependencies and run tests
- Build the Node.js app
- SSH into the specified EC2 instance
- Pull latest code, install dependencies, and restart the app using PM2
- Notify your team via Slack with success or failure status

### EC2 One-Time Setup

```bash
sudo npm install -g pm2
cd /home/ubuntu/your-app
pm2 start npm --name "app" -- start
pm2 save
```

## Notes

- Ensure your EC2 Security Group allows SSH access from GitHub Actions IPs (or temporarily 0.0.0.0/0 for testing with key-based auth)

- The repo should already be cloned into the specified path on each EC2 instance

- You can customize this pipeline to support Docker, auto-tagging, or rollback features
