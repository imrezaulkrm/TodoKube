# Use the official Node.js image as the base image
FROM node:current-alpine3.19 AS build

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Set npm timeout to avoid network issues
#RUN npm config set timeout 60000

# Install dependencies
RUN npm install

# Copy the rest of the application code to the working directory
COPY . .

# Set NODE_OPTIONS to use the legacy OpenSSL provider
ENV NODE_OPTIONS=--openssl-legacy-provider

# Build the React application
RUN npm run build

# Stage 2: Serve the app with a lightweight web server
FROM nginx:alpine

# Copy the build output to the Nginx HTML directory
COPY --from=build /app/build /usr/share/nginx/html


# Expose port 80 to the outside world
EXPOSE 80

# Start Nginx when the container launches
CMD ["nginx", "-g", "daemon off;"]
