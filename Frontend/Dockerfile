FROM node:16

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY ./package*.json ./

# Install app dependencies
RUN npm install

# Bundle app source
COPY . ./

# Ensure vue-cli-service is executable
RUN chmod +x /app/node_modules/.bin/vue-cli-service

# Ensure all shell scripts are executable
RUN find /app -type f -name "*.sh" -exec chmod +x {} \;

# Expose port
EXPOSE 8080

# Start command
CMD ["npm", "run", "serve"]
