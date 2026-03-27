# Railway-compatible DeerFlow Backend Dockerfile
FROM python:3.12-slim-bookworm

ARG NODE_MAJOR=22

# Install system dependencies + Node.js + uv
RUN apt-get update && apt-get install -y \
    curl \
    build-essential \
    gnupg \
    ca-certificates \
    && mkdir -p /etc/apt/keyrings \
    && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg \
    && echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_${NODE_MAJOR}.x nodistro main" > /etc/apt/sources.list.d/nodesource.list \
    && apt-get update \
    && apt-get install -y nodejs \
    && rm -rf /var/lib/apt/lists/*

# Install uv via pip (avoids ghcr.io multi-stage which Railway can't reach)
RUN pip install uv

# Set working directory
WORKDIR /app

# Copy source code
COPY backend ./backend

# Install dependencies
RUN cd backend && uv sync

# Expose port
EXPOSE 8001

# Default command
CMD ["sh", "-c", "cd backend && PYTHONPATH=. uv run uvicorn app.gateway.app:app --host 0.0.0.0 --port 8001"]
