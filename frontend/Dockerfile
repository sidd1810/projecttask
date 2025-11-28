# frontend/Dockerfile
# Stage 1: build Angular app
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
# Build - adjust if your project uses a different script
RUN npm run build -- --configuration=production || npm run build

# Stage 2: serve with nginx
FROM nginx:stable-alpine
COPY --from=builder /app/dist /usr/share/nginx/html
# Remove default conf and use static config
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx-static.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
