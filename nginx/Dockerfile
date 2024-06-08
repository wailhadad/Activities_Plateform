# Use a stable and specific version of Node rather than the 'latest' version
FROM node:16-alpine as builder

WORKDIR /vue-ui

# Copy package.json and package-lock.json for better cache management
COPY app/package*.json ./

# Install dependencies using npm ci, which is better for production builds
RUN npm ci

# Copy the rest of the application source code
COPY app/ ./

# Build the application
RUN npm run build

# NGINX server to serve the Vue.js application
FROM nginx:stable-alpine

# Copy custom NGINX configuration
COPY nginx/nginx.conf /etc/nginx/nginx.conf

# Remove default NGINX files and copy the built Vue.js files
RUN rm -rf /usr/share/nginx/html/*
COPY --from=builder /vue-ui/dist /usr/share/nginx/html

# Expose port 80 for HTTP traffic
EXPOSE 80

# Set the entry point to start NGINX in the foreground
ENTRYPOINT ["nginx", "-g", "daemon off;"]
