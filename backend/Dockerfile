# Railway-compatible DeerFlow Backend Dockerfile
# Uses multi-stage to keep final image small
# Stage 1: Install dependencies
FROM python:3.12-slim-bookworm AS builder

WORKDIR /app
COPY backend/pyproject.toml backend/uv.lock* backend/

RUN pip install --no-cache-dir uv \
    && cd backend \
    && uv sync 2>/dev/null || uv sync --no-dev 2>/dev/null || pip install -r requirements.txt 2>/dev/null || true

# Stage 2: Runtime
FROM python:3.12-slim-bookworm

WORKDIR /app
COPY --from=builder /app /app
COPY --from=builder /usr/local/lib/python3.12 /usr/local/lib/python3.12
COPY --from=builder /usr/local/bin /usr/local/bin
COPY backend ./backend

EXPOSE 8001

CMD ["sh", "-c", "cd backend && PYTHONPATH=. python -m uvicorn app.gateway.app:app --host 0.0.0.0 --port 8001"]
