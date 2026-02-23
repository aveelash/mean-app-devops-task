# Use official Node image
FROM node:18

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy remaining code
COPY . .

# Expose backend port
EXPOSE 8080

# Start backend
CMD ["npm", "start"]