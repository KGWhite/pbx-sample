FROM node:25.9.0-alpine3.22 AS builder

WORKDIR /app

# Copy package files if they exist
COPY package*.json ./
RUN if [ -f package.json ]; then npm install; fi

# Copy the rest of the application
COPY . .

# Build the React application
RUN if [ -f package.json ]; then npm run build; else mkdir build && echo "No react app found" > build/index.html; fi

FROM nginx:1.29.8-alpine
# Copy the custom Nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy the built React app to NGINX HTML directory
COPY --from=builder /app/build /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
