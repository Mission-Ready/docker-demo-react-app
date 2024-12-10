# Dockerizing a React App

Follow these steps to dockerize your React application:

## 1. Create a `.dockerignore` File
Create a `.dockerignore` file in the root of your project to exclude unnecessary files from the Docker image.

```plaintext
node_modules
```

## 2. Create a `Dockerfile`
Create a `Dockerfile` in the root of your project to define the Docker image.

```Dockerfile
# Use an official Node.js runtime as a parent image
FROM node:20

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the React app
RUN npm run build

# Use an official Nginx image to serve the React app
FROM nginx:alpine

# Copy the build output to the Nginx HTML directory
COPY --from=0 /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
```

## 3. Build the Docker Image
Run the following command to build the Docker image:

```sh
docker build -t my-react-app .
```

## 4. Run the Docker Container
Run the following command to start a container from the Docker image:

```sh
# Run the container and map port 4040 to port 80 on the host
# Now, 4040 is the port on the your computer that will be used to access the React app in port 80 in the container
# localhost:4040 gives you access to the app running in the container 
docker run -p 4040:80 my-react-app
```

Your React app should now be accessible at `http://localhost` since we've use port 80.
