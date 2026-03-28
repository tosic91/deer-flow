# Railway-compatible DeerFlow Backend Dockerfile
# Slimmed down to stay under Railway 2GB build memory limit
FROM python:3.12-slim-bookworm

# Install minimal system dependencies (no Node.js, no build-essential)
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Install uv via official installer (lighter than pip install)
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.local/bin:$PATH"

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
