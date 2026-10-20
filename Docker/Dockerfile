
# Use Nginx as the base image
FROM nginx:alpine

# Copy website files into the container image
COPY . /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start the Nginx web server
CMD ["nginx", "-g", "daemon off;"]
