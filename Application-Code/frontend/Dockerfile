# FROM node:14
# WORKDIR /usr/src/app
# COPY package*.json ./
# RUN npm install
# COPY . .
# CMD [ "npm", "start" ]

# Stage 1: Build stage
FROM node:14 AS builder
WORKDIR /usr/src/app

# Copy package.json and package-lock.json to install dependencies
COPY package*.json ./
RUN npm install --production

# Copy the application code
COPY . .

# Optional: Run any build steps if necessary (e.g., for TypeScript, transpiling, etc.)
# RUN npm run build

# Stage 2: Production stage (distroless)
FROM gcr.io/distroless/nodejs:14

# Set working directory
WORKDIR /usr/src/app

# Copy only the necessary files from the build stage
COPY --from=builder /usr/src/app /usr/src/app

# Use distroless entrypoint (npm start)
CMD ["npm", "start"]


