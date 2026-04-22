# GitHub Actions Setup

This project uses GitHub Actions to automatically build and push Docker images to Docker Hub.

## Required Secrets

To enable the Docker build and publish workflow, add the following secrets to your GitHub repository:

### Steps:
1. Go to your repository on GitHub
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret** and add:

| Secret Name | Value |
|-------------|-------|
| `DOCKERHUB_USERNAME` | Your Docker Hub username (e.g., `adamsardo`) |
| `DOCKERHUB_TOKEN` | Your Docker Hub Personal Access Token |

### How to create a Docker Hub Personal Access Token:

1. Visit https://hub.docker.com/settings/security
2. Click **New Access Token**
3. Give it a name (e.g., "GitHub Actions")
4. Select **Read, Write** permissions
5. Copy the token and paste it into the GitHub secret

## Workflow Triggers

The workflow automatically builds and pushes images on:

- **Push to `main` branch** → Tags as `latest` + `main`
- **Git tags** (e.g., `v1.0.0`) → Tags as `v1.0.0`, `1.0`, and `latest`
- **Pull requests to `main`** → Builds only (no push)

## Image Tags

When you push a tag like `v1.0.0`, the following tags are created:
- `adamsardo/discord-mcp:v1.0.0` (full version)
- `adamsardo/discord-mcp:1.0` (major.minor)
- `adamsardo/discord-mcp:latest` (if on main branch)

## Monitoring Builds

1. Push a commit or tag to `main` branch
2. Go to your repository → **Actions** tab
3. Click the workflow run to view logs

## Local Testing

Test the Docker build locally before committing:

```bash
docker buildx build --load -t adamsardo/discord-mcp:test .
docker run -e DISCORD_TOKEN=<token> adamsardo/discord-mcp:test
```
