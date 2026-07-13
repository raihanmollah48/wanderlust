# Stage 1
FROM node:21 AS backend-builder

# setup the working dir
WORKDIR /app

# Copy package files first for better layer caching
COPY package*.json ./

# Copy pre-installed node_modules (from CI pipeline) or empty if not pre-installed
COPY node_modules ./

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
