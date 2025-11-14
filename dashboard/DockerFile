# Stage 1 — buildaa Next.js-dashin
FROM node:22-alpine AS builder

# Työhakemisto kontissa
WORKDIR /app

# Kopioi package.json + lockfile (jos myöhemmin tulee package-lock.json)
COPY package*.json ./

# Asenna riippuvuudet (dev + prod)
RUN npm ci

# Kopioi loput dashboard-koodista
COPY . .

# Rakenna tuotantoversio
RUN npm run build

# Poista dev-depsit, jotta runtime on kevyt
RUN npm prune --omit=dev


# Stage 2 — runtime-image
FROM node:22-alpine

WORKDIR /app

ENV NODE_ENV=production
ENV PORT=3000

# Kopioi rakennettu appi ja prod-riippuvuudet builderista
COPY --from=builder /app ./

EXPOSE 3000

# Käynnistä Next.js tuotantomoodissa
CMD ["npm", "start"]
