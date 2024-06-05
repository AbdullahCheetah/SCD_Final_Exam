# Base image
FROM node:14-alpine

# Set working directory
WORKDIR /21i_1230_backend

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy application code
COPY . .

# Expose port 3230
EXPOSE 3233

# Start the application
CMD ["node", "index.js"]
