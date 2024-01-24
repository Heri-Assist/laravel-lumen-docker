# node image version 
FROM node:20-alpine as builder

ENV NODE_VERSION 20.5.0

# Create app directory
WORKDIR /app
COPY ./ /app/

# Install app dependencies
ENV NODE_OPTIONS="--max-old-space-size=8192"
RUN npm install
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist/ /usr/share/nginx/html

