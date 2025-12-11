# -------- Build Stage --------
FROM node:20 AS builder
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install --force

# Copy the rest of the Angular project (src/, angular.json, etc.)
COPY . .

# Build Angular app
RUN npm run build

# -------- Runtime Stage --------
FROM nginx:alpine

# remove default nginx content
RUN rm -rf /usr/share/nginx/html/*

# copy built Angular app from build stage
COPY --from=builder /app/dist/angular-conduit/ /usr/share/nginx/html/

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
