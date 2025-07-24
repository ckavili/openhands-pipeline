# OpenHands Pipeline

This directory contains Kubernetes manifests for running OpenHands issue resolution pipelines using Tekton.

## Files

### Core Pipeline Files

- **`openhands-issue-resolver-pipeline.yaml`** - Main Tekton pipeline that processes GitHub issues using OpenHands AI agent
  - Clones repository from GitHub
  - Runs OpenHands issue resolver to analyze and fix issues
  - Creates draft pull requests with solutions

### Security & RBAC

- **`secrets.yaml`** - Kubernetes Secret containing sensitive credentials
  - `llm-api-key`: API key for LLM provider (OpenRouter/OpenAI)
  - `pat-token`: GitHub Personal Access Token for repository access

- **`pipeline-role.yaml`** - RBAC Role defining permissions for pipeline execution
  - Manages pods (create, get, list, watch, delete, patch, update)
  - Manages persistent volume claims

- **`pipeline-rolebinding.yaml`** - Binds the pipeline role to the pipeline service account

## Pipeline Parameters

The pipeline accepts these key parameters:
- `repo-url`: GitHub repository URL to process
- `issue-number`: Specific issue number to resolve
- `llm-model`: AI model to use (default: Qwen)
- `max-iterations`: Maximum processing iterations
- `username`: GitHub username for authentication

## Usage

1. Update `secrets.yaml` with your actual API keys and tokens
2. Apply the RBAC manifests: `kubectl apply -f pipeline-role.yaml pipeline-rolebinding.yaml`
3. Apply secrets: `kubectl apply -f secrets.yaml`
4. Apply the pipeline: `kubectl apply -f openhands-issue-resolver-pipeline.yaml`