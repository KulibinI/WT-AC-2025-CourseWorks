FROM node:20-slim

WORKDIR /app

RUN apt-get update && apt-get install -y openssl ca-certificates && rm -rf /var/lib/apt/lists/*

COPY package*.json ./
COPY prisma ./prisma/

RUN npm ci

COPY . .

RUN npx prisma generate
RUN npm run build

RUN cp src/swagger.json dist/src/ 2>/dev/null || true

EXPOSE 3000

CMD sh -c "npx prisma migrate deploy && npm run seed && node dist/src/server.js"