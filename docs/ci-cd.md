# CI / CD — Build and push Docker images to Docker Hub

This repo now contains a GitHub Actions workflow that builds and pushes Docker images for both the backend and frontend to Docker Hub.

Workflow file:

- `.github/workflows/docker-build-and-push.yml`

Trigger:

- Push to `main` branch
- Manual: `workflow_dispatch` (you can run this from GitHub Actions UI)

Which images are pushed:

- `${{ secrets.DOCKERHUB_USERNAME }}/weather-backend:latest` and `${{ secrets.DOCKERHUB_USERNAME }}/weather-backend:<sha>`
- `${{ secrets.DOCKERHUB_USERNAME }}/weather-frontend:latest` and `${{ secrets.DOCKERHUB_USERNAME }}/weather-frontend:<sha>`

Required GitHub repository secrets

Add the following repository secrets in GitHub (Settings → Secrets → Actions):

- `DOCKERHUB_USERNAME` — your Docker Hub username (or organization name)
- `DOCKERHUB_PASSWORD` — Docker Hub access token or account password (access tokens are recommended)

Optional / notes

- The workflow builds multi-arch images (linux/amd64 and linux/arm64) using Docker Buildx.
- The workflow builds multi-arch images (linux/amd64 and linux/arm64) using Docker Buildx.
	- Note: GitHub-hosted runners use a Docker driver that doesn't support multi-arch builds by default. The workflow now creates a dedicated buildx builder which uses the docker-container driver and QEMU (see `.github/workflows/docker-build-and-push.yml`) so multi-platform builds succeed.
- Make sure the Dockerfile paths are `frontend/Dockerfile` and `backend/Dockerfile` (these are already present in the repo).
- If you want different image names, set them either by editing the workflow or by storing the repo name in a new secret (e.g. `DOCKERHUB_REPO_BACKEND`).

How to test manually

1. Push your branch to `main` (or open a PR and then merge), or go to the repository → Actions → Build & Push Docker images → Run workflow.
2. Select `workflow_dispatch` and click Run workflow.
3. Watch the Actions logs — you should see Docker login, build and push steps for both jobs.

If anything fails, paste the failing step logs and I can help diagnose further.
