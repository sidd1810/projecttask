# backend/Dockerfile
FROM node:18-alpine

WORKDIR /usr/src/app

# Install only production deps for smaller image
COPY package*.json ./
RUN npm ci --only=production

COPY . .

ENV PORT=3000
EXPOSE 3000

CMD ["node", "server.js"]

