# Stage 1
FROM node:21 AS backend-builder

# setup the working dir
WORKDIR /app

# Copy package files first for better layer caching
COPY package*.json ./

# Install dependencies with retry config for flaky networks
RUN npm config set fetch-retries 5 && \
    npm config set fetch-retry-mintimeout 20000 && \
    npm config set fetch-retry-maxtimeout 120000 && \
    npm ci

# code
COPY . .

# tests
RUN npm run test

# Stage 2
FROM node:21-slim

# setup the working dir
WORKDIR /app

# copy the above stage as compressed
COPY --from=backend-builder /app .

COPY .env.docker .env

# Port
EXPOSE 5000

# App
CMD ["npm", "start"]
