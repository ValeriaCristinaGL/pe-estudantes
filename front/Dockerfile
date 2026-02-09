# Stage 1: Build the Next.js application
FROM node:24.4-alpine AS builder

WORKDIR /app

# Build arg para injetar a URL da API no build time (NEXT_PUBLIC_ exige isso)
ARG NEXT_PUBLIC_API_EDU_TRACE
ENV NEXT_PUBLIC_API_EDU_TRACE=${NEXT_PUBLIC_API_EDU_TRACE}

COPY package.json package-lock.json* ./
RUN npm install --frozen-lockfile

COPY . .

RUN npm run build

# Stage 2: Production image
FROM node:24.4-alpine AS runner

WORKDIR /app

# Deixe os envs injetados via docker-compose
ENV NEXT_TELEMETRY_DISABLED=1
ENV NODE_ENV=production

COPY --from=builder /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./package.json

EXPOSE 3000

CMD ["npm", "start"]
