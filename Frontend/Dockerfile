# syntax=docker/dockerfile:1

ARG NODE_VERSION=18.17.1

FROM node:${NODE_VERSION}-alpine

# Use production node environment by default.
ENV NODE_ENV production

# Create app directory
WORKDIR /usr/src/app

# Copy package files before setting USER
COPY package*.json ./

# Install dependencies
RUN --mount=type=cache,target=/root/.npm \
    npm ci --omit=dev

# Copy the rest of the source code
COPY . .

# Ensure node user owns the app directory (especially node_modules)
RUN chown -R node:node /usr/src/app

# Switch to non-root user
USER node

# Expose the port the app runs on
EXPOSE 3000

# Start the app
CMD ["npm", "start"]
