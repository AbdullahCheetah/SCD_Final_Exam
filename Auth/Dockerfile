# Base image
FROM node:14-alpine

# Set working directory
WORKDIR /21i_1230_backend

# Install dependencies
COPY package.json package-lock.json ./
RUN npm install

# Copy application code
COPY . .

# Expose port 3230
EXPOSE 3230

# Start the application
CMD ["node", "index.js"]
