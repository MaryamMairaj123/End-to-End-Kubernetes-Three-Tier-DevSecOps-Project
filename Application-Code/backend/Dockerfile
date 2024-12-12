# FROM node:14
# WORKDIR /usr/src/app
# COPY package*.json ./
# RUN npm install
# COPY . .
# CMD ["node", "index.js"]

# Stage 1: Build stage
FROM node:14 AS builder
WORKDIR /usr/src/app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install --production

# Copy the rest of the application code
COPY . .

# Build the application (optional if you need to build assets or compile TypeScript)
# RUN npm run build

# Stage 2: Production stage (distroless)
FROM gcr.io/distroless/nodejs:14

# Copy only the necessary files from the build stage
WORKDIR /usr/src/app
COPY --from=builder /usr/src/app /usr/src/app

# Use distroless entrypoint
CMD ["index.js"]
