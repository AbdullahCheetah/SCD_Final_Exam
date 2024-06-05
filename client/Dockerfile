# Base image
FROM node:14-alpine

# Set working directory
WORKDIR /21i_1230_frontend

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy project files
COPY . .

# Build the frontend assets
RUN npm run build

# Expose port 1230
EXPOSE 1230

# Start the application
CMD ["npx", "serve", "-s", "build", "-l", "1230"]
