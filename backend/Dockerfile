FROM node:16-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci --legacy-peer-deps

COPY . .

RUN npm run build

FROM node:16-alpine AS runner

ENV NODE_ENV=production

RUN npm install -g pm2

WORKDIR /app

COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/ecosystem.config.js ./ecosystem.config.js

EXPOSE 3000

CMD ["pm2-runtime", "ecosystem.config.js"]