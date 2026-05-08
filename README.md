# market-hub

A financial data portal sourcing live data from open APIs. Built with React (frontend) and Node.js Lambda functions (backend), hosted on AWS.

## Architecture

```
                   Route 53 (custom domain)
                          │
                     S3 Static Site
                    (React frontend)
                          │
                   API Gateway (HTTP API)
                          │
              ┌───────────┴───────────┐
        Lambda: github-repos   Lambda: asx-equities
              │                       │
        GitHub REST API         yahoo-finance2
```

## Data Sources

| Feature | API | Auth |
|---------|-----|------|
| GitHub Top Repos | GitHub REST API | Optional token (raises rate limit) |
| ASX Top Equities | yahoo-finance2 (npm) | None required |

## Project Structure

```
market-hub/
├── frontend/                  # React + Vite SPA
│   └── src/
├── backend/
│   └── functions/
│       ├── github-repos/      # Lambda: GET /github-repos
│       └── asx-equities/      # Lambda: GET /asx-equities
├── infrastructure/
│   └── template.yaml          # AWS SAM template
└── .github/
    └── workflows/
        ├── deploy-backend.yml
        └── deploy-frontend.yml
```

## Tech Stack

- **Frontend:** React 18, Vite, React Router v6, Tailwind CSS
- **Backend:** Node.js 20, AWS Lambda, API Gateway (HTTP API)
- **Infrastructure:** AWS SAM (CloudFormation)
- **Hosting:** S3 static website + Route 53 (custom domain)
- **CI/CD:** GitHub Actions

## Local Development

### Prerequisites
- Node.js 20+
- AWS CLI + AWS SAM CLI
- GitHub CLI (`gh`)

### Frontend
```bash
cd frontend
npm install
cp .env.example .env.local   # set VITE_API_BASE_URL
npm run dev
```

### Backend (local with SAM)
```bash
cd infrastructure
sam build
sam local start-api
```

## Deployment

Deployments are automated via GitHub Actions on push to `main`.

### Required GitHub Secrets

| Secret | Description |
|--------|-------------|
| `AWS_ACCESS_KEY_ID` | AWS IAM access key |
| `AWS_SECRET_ACCESS_KEY` | AWS IAM secret key |
| `AWS_REGION` | AWS region (e.g. `ap-southeast-2`) |
| `S3_BUCKET_NAME` | S3 bucket for frontend |
| `VITE_API_BASE_URL` | API Gateway base URL |
| `GITHUB_TOKEN_API` | GitHub token for Lambda (optional, raises rate limit) |

## Issues / Roadmap

Track progress via [GitHub Issues](https://github.com/radhikavm/market-hub/issues).
