# Railway-compatible DeerFlow Backend Dockerfile
# Ultra-lean: no apt-get to avoid Railway build timeout/cancellation
FROM python:3.12-slim-bookworm

# Install uv directly via pip (no curl needed)
RUN pip install --no-cache-dir uv

# Set working directory
WORKDIR /app

# Copy source code
COPY backend ./backend

# Install dependencies
RUN cd backend && uv sync --no-dev 2>/dev/null || cd backend && uv sync

# Expose port
EXPOSE 8001

# Default command
CMD ["sh", "-c", "cd backend && PYTHONPATH=. uv run uvicorn app.gateway.app:app --host 0.0.0.0 --port 8001"]
