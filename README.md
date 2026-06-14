# Citta - Zero-Waste Agentic AI Platform

Welcome to the Citta Docker Compose Distribution. This repository is designed to help you quickly deploy and run the Citta Agentic AI Platform on your local machine or server using pre-built Docker images.

For the source code, please visit the [Citta-Core repository](https://github.com/pphothidaen/citta-core).

## System Requirements

- **Docker Engine** 24.0+ and **Docker Compose** V2
- **RAM**: 8GB minimum (16GB recommended for local LLM inference)
- **Disk**: 20GB+ available space (Docker images + models + Vector DB)

## Architecture Overview

This distribution runs a multi-node architecture locally:
- **NATS JetStream**: The event bus connecting all services.
- **ChromaDB**: The memory fabric (semantic cache and artifacts).
- **OpenVINO OVMS**: Local inference provider serving Qwen-1.5B by default.
- **Cognitive Kernel**: The central API gateway (`:8002`).
- **Inference Mesh**: Executes model requests.
- **Governance**: Quality gate and evaluation (`:8004`).
- **Learning Factory**: Distillation loop for continuous learning.

## Quick Start (Local-Only Mode)

By default, Citta is configured to run in "Local-Only Mode" using OpenVINO for inference. No external API keys are required.

1. **Clone this repository**
   ```bash
   git clone https://github.com/pphothidaen/CittaProject.git
   cd CittaProject
   ```

2. **Configure environment variables**
   ```bash
   cp .env.example .env
   ```
   *(Optional) You can open `.env` and configure external API keys (Google, OpenAI, etc.) if you wish to use them.*

3. **Start the platform**
   ```bash
   docker compose up -d
   ```
   *Note: The first time you run this, it will download several gigabytes of Docker images and the OpenVINO model. Please be patient.*

4. **Verify the deployment**
   Wait about 30 seconds for all services to start, then check the Kernel API:
   ```bash
   curl http://localhost:8002/health
   ```
   You can access the API documentation at `http://localhost:8002/docs`.

## Stopping the Platform

To stop the platform without losing your data:
```bash
docker compose down
```

To stop and wipe all data (cache, artifacts, etc.):
```bash
docker compose down -v
```
