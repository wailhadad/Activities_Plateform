FROM node:16

WORKDIR /app

# Install Vue CLI globally
RUN npm install -g @vue/cli

COPY package*.json ./

RUN npm install

COPY . .

# Ensure permissions
RUN chmod -R 777 /app

EXPOSE 8080
CMD ["npm", "run", "serve"]