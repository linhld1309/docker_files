# Use the official Node.js 16.x LTS image as the base image
FROM node:16.18.0

# Set the working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code to the container
COPY . .

# Expose the port that your NestJS app listens on (adjust the port if needed)
EXPOSE 3000

# Start the NestJS application
CMD [ "npm", "run", "start:prod" ]
