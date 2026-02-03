# =========================
# Stage 2: Runtime stage
# =========================
FROM python:3.11-slim AS runner

WORKDIR /app   # ✅ THIS LINE WAS MISSING

RUN adduser --disabled-password --gecos '' appuser && \
    chown -R appuser:appuser /app

COPY --from=builder /install /usr/local
COPY . .

USER appuser

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
