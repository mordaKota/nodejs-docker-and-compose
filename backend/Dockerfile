# Этап 1: Сборка
FROM node:16-alpine AS builder

# Рабочая директория
WORKDIR /app

# Копируем package.json и package-lock.json
COPY package*.json ./

# Устанавливаем зависимости
RUN npm ci --legacy-peer-deps

# Копируем весь проект и выполняем сборку
COPY . .
RUN npm run build

# Этап 2: Минимизированное окружение для запуска
FROM node:16-alpine AS runner

# Устанавливаем переменную окружения для production
ENV NODE_ENV=production

# Устанавливаем PM2
RUN npm install -g pm2

# Рабочая директория
WORKDIR /app

# Копируем готовые файлы из builder
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/ecosystem.config.js ./ecosystem.config.js

# Открываем порт
EXPOSE 3004

# Команда для запуска
CMD ["pm2-runtime", "ecosystem.config.js"]