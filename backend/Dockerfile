FROM node:21 AS backend-builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm test

FROM node:21-slim

WORKDIR /app

COPY --from=backend-builder /app .

COPY .env.docker .env

EXPOSE 5000

CMD ["npm", "start"]
