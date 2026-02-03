# =========================
# Stage 1: Build stage
# =========================
FROM node:20-slim AS builder

WORKDIR /app

# Copy package files
COPY package.json package-lock.json ./

# Install dependencies
RUN npm ci

# Copy source code
COPY . .

# Build Next.js (standalone output)
RUN npm run build


# =========================
# Stage 2: Production stage
# =========================
FROM node:20-slim AS runner

WORKDIR /app

# Create non-root user
RUN groupadd -g 1001 nodejs && \
    useradd -m -u 1001 -g nodejs nextjs

# Copy standalone build output
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

# Fix permissions
RUN chown -R nextjs:nodejs /app

# Use non-root user
USER nextjs

# Expose Next.js port
EXPOSE 3000

# Disable telemetry
ENV NEXT_TELEMETRY_DISABLED=1
ENV PORT=3000

# Start standalone server
CMD ["node", "server.js"]
