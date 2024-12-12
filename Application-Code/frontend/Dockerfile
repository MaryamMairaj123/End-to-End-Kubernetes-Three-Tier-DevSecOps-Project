# FROM node:14
# WORKDIR /usr/src/app
# COPY package*.json ./
# RUN npm install
# COPY . .
# CMD [ "npm", "start" ]

# Stage 1: Build stage
FROM node:14 AS builder

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy package.json and package-lock.json to install dependencies
COPY package*.json ./

# Install all dependencies (production and development)
RUN npm install

# Copy the application code
COPY . .

# Optional: Run any build steps if necessary (e.g., for React, TypeScript, etc.)
# RUN npm run build

# Stage 2: Production stage (distroless)
FROM gcr.io/distroless/nodejs:14

# Set the working directory
WORKDIR /usr/src/app

# Copy only the necessary files from the build stage
COPY --from=builder /usr/src/app /usr/src/app

# Expose the port your app runs on
EXPOSE 3000

# Set the command to start your app
CMD ["npm", "start"]



