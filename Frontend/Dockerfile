# Stage 1: Build stage
FROM node:16 as build-stage

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install app dependencies
RUN npm install

# Bundle app source
COPY . ./

# Ensure vue-cli-service is executable
RUN chmod +x /app/node_modules/.bin/vue-cli-service

# Ensure all shell scripts are executable
RUN find /app -type f -name "*.sh" -exec chmod +x {} \;

# Set permissions (if needed on Linux)
RUN chown -R node:node /app

# Build the app
RUN npm run build

# Stage 2: Production stage
FROM nginx:latest

# Copy the build output to Nginx's html directory
COPY --from=build-stage /app/dist/ /usr/share/nginx/html

# Copy custom Nginx configuration
COPY ../nginx/nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
