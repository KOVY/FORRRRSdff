# Installation Guide for Autonomous Development Workflow

## Overview
This guide provides step-by-step instructions to set up a local-first, autonomous AI development environment on Ubuntu, leveraging a 36GB VRAM GPU. The system enables a "Zero to App" workflow with persistent memory, powered by Qwen2.5 70B, Qwen2.5-Coder, Agent Zero Reasoning (AZR), Graph-Code, and VS Code with Continue.dev.

## Prerequisites
- **OS**: Ubuntu 20.04 or 22.04
- **Hardware**: NVIDIA GPU with 36GB VRAM
- **Internet**: Stable connection for downloading dependencies
- **Google Drive**: Folder named "Autonomous_Development_Workflow" for storing project files

## Step 1: Install System Dependencies
1. Update system packages:
   ```bash
   sudo apt update
   sudo apt install -y nvidia-driver-535 nvidia-cuda-toolkit
   ```
2. Verify GPU:
   ```bash
   nvidia-smi
   ```

## Step 2: Install Python Dependencies
1. Install required Python packages:
   ```bash
   pip install vllm crewai langgraph mem0 robocorp glass fastapi uvicorn semgrep bandit diffusers transformers torch
   ```
2. Install Trivy for security scanning:
   ```bash
   curl -sSfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh
   ```

## Step 3: Set Up vLLM for Language Models
1. Start vLLM servers for Qwen2.5 models:
   ```bash
   vllm serve qwen2.5:70b --host 0.0.0.0 --port 8000 &
   vllm serve qwen2.5-coder --host 0.0.0.0 --port 8001 &
   ```

## Step 4: Install Graph-Code
1. Clone and set up Graph-Code for code analysis:
   ```bash
   git clone https://github.com/vitali87/code-graph-rag.git
   cd code-graph-rag
   uv sync --extra treesitter-full
   docker-compose up -d
   cp .env.example .env
   cd ..
   ```
2. Update `.env` in `code-graph-rag` with values from your project's `.env` (see below).

## Step 5: Install VS Code and Extensions
1. Install VS Code:
   ```bash
   sudo snap install code --classic
   ```
2. Install extensions:
   ```bash
   code --install-extension Continue.continue
   code --install-extension Codium.codium
   ```
3. Configure VS Code for Continue.dev:
   ```json
   // .vscode/settings.json
   {
       "continue.model": "vllm/qwen2.5:70b",
       "continue.apiBaseUrl": "http://localhost:8000",
       "continue.secondaryModel": "vllm/qwen2.5-coder"
   }
   ```

## Step 6: Clone Context-Engineering
1. Clone the Context-Engineering repository for templates:
   ```bash
   git clone https://github.com/davidkimai/Context-Engineering
   ```

## Step 7: Set Up Project Structure
1. Create the project directory:
   ```bash
   mkdir -p autonomous_dev_workflow
   cd autonomous_dev_workflow
   ```
2. Initialize the folder structure:
   ```bash
   mkdir -p project_memory ai-framework/core ai-framework/agents ai-framework/protocols ai-framework/reasoning
   mkdir -p web-integration/mcp-b web-integration/wesql web-integration/testing
   mkdir -p workflow-engine/phases workflow-engine/orchestration workflow-engine/quality-control
   mkdir -p deployment/railway deployment/vercel deployment/docker deployment/wesql-deploy
   mkdir -p quick-start templates/full-stack-app templates/ai-enhanced-frontend templates/multi-app-workflow templates/enterprise-architecture
   mkdir -p docs .github/workflows
   ```

## Step 8: Configure Environment
1. Create and populate the `.env` file in the project root (see `.env` artifact below).
2. Copy relevant `.env` values to `code-graph-rag/.env`.

## Step 9: Run Setup Script
1. Execute the setup script to finalize installation:
   ```bash
   bash quick-start/setup.sh
   ```
Zjednodušit paměť:
Proč: Váš systém .md + Mem0 je robustní, ale správa více .md souborů může být náročná při velkých projektech. Udacity navrhuje vektorové databáze, které jsou škálovatelnější.
Řešení: Přidat ChromaDB jako doplňkový vektorový backend k Mem0, aby se zjednodušilo vyhledávání a škálování paměti:
bash

Collapse

Wrap

Run

Copy
pip install chromadb
Upravit memory_manager.py pro integraci ChromaDB.
Převzít Pydantic pro JSON:
Proč: Glass je skvělý pro strukturované výstupy, ale Pydantic (z Udacity) je standardem pro Python a zjednodušuje validaci JSON.
Řešení: Integrovat Pydantic do orchestrator.py pro výstupy agentů:
python

Collapse

Wrap

Run

Copy
from pydantic import BaseModel
class WorkResult(BaseModel):
    completed_work: dict
    new_state: ProjectState
    next_suggestions: list
    quality_metrics: dict
## Step 10: Upload to Google Drive
1. Install `gdrive` CLI:
   ```bash
   sudo snap install gdrive
   gdrive auth
   ```
2. Find the Google Drive folder ID for "Autonomous_Development_Workflow":
   ```bash
   gdrive list
   ```
3. Upload the project:
   ```bash
   gdrive push -destination <GOOGLE_DRIVE_FOLDER_ID> .
   ```

## Step 11: Verify Setup
1. Start FastAPI server:
   ```bash
   uvicorn main:app --reload
   ```
2. Test with a prompt via curl:
   ```bash
   curl -X POST -d '{"prompt": "Vytvoř mi e-commerce platformu"}' http://localhost:8002/run
   ```

3. Check VS Code Continue.dev integration by entering a prompt like "Kde jsme teď?".

## Troubleshooting
- **GPU Issues**: Ensure NVIDIA drivers are installed (`nvidia-smi`).
- **vLLM Errors**: Check ports 8000 and 8001 are free (`netstat -tuln`).
- **Graph-Code**: Verify Docker is running (`docker ps`).
- **Google Drive**: Ensure correct folder ID and sufficient storage.

## Next Steps
- Start development: "Vytvoř mi e-commerce platformu s uživatelskou registrací."
- Resume work: "Pokračuj od bodu 2.1.2."
- Request improvements: "Jaké máš AZR návrhy na vylepšení?"

---
**Date**: July 25, 2025  
