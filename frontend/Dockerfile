# =========================
# Stage 1: Builder
# =========================
FROM python:3.11-slim AS builder

WORKDIR /app

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    curl \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt


# =========================
# Stage 2: Runner
# =========================
FROM python:3.11-slim AS runner

WORKDIR /app   # ✅ THIS CREATES /app (CRITICAL)

RUN adduser --disabled-password --gecos '' appuser && \
    chown -R appuser:appuser /app

COPY --from=builder /install /usr/local
COPY . .

USER appuser

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
