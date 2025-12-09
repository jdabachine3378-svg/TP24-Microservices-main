# ---------- Stage 1 : build Angular ----------
FROM node:14.15.0-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build -- --configuration=production

# ---------- Stage 2 : servir avec Nginx ----------
FROM nginx:alpine

# Copier les fichiers buildés
COPY --from=builder /app/dist/smart-home /usr/share/nginx/html

# Copier la configuration Nginx personnalisée
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
