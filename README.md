### Dockerfile

```Dockerfile
# Use an official Node.js image to build the React app
FROM node:18 AS build

# Set the working directory
WORKDIR /app

# Install git
RUN apt-get update && apt-get install -y git

# Clone the GitHub repository (replace with your repo URL)
RUN git clone https://github.com/username/repo-name.git .

# Install the dependencies
RUN npm install

# Build the React app for production
RUN npm run build

# Use an official Nginx image to serve the app
FROM nginx:alpine

# Copy the build files from the previous stage to the Nginx public directory
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80 to the outside world
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
```



4. **Run the Docker Container**: After building the image, run the container with the following command:
   ```bash
   docker run -p 80:80 react-nginx-app
   ```

5. **Access the Application**: Open your browser and navigate to `http://localhost` to see your React application running on Nginx.

### Notes:
- Ensure that your GitHub repository is public or that you have access credentials set up for private repositories if needed.
- Make sure the Docker build context doesn’t include sensitive information like GitHub credentials if you’re working with private repos.
