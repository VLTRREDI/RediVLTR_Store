# Stage 1: installa tutte le dipendenze (questo layer viene cachato da Railway)
FROM node:20-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --no-audit --no-fund

# Stage 2: build del progetto Medusa
FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# Stage 3: immagine finale leggera con solo il server buildato
FROM node:20-alpine AS runner
WORKDIR /app
COPY --from=builder /app/.medusa/server ./
RUN npm install --omit=dev --no-audit --no-fund

EXPOSE 9000
CMD ["sh", "-c", "npm run predeploy && npm run start"]