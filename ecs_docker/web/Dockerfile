FROM nginx:1

# Set environment variables
ENV TZ=Asia/Tokyo

# Copy nginx configuration files
COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./default.conf /etc/nginx/conf.d/
