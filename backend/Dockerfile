FROM node:18-alpine

WORKDIR /usr/src/app

COPY package*.json ./

# FIX: use npm install instead of npm ci
RUN npm install --production

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]

