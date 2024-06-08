FROM node:16

# Set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY ./package*.json ./

# Install app dependencies
RUN npm install

# Install Vue CLI globally
RUN npm install -g @vue/cli

# Bundle app source
COPY . ./

# Set permissions (if needed on Linux)
RUN chown -R .

# Expose port
EXPOSE 8080

# Start command
CMD ["npm", "run", "serve"]
