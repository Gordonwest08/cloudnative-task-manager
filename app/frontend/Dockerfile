FROM node:20.19-alpine AS builder

WORKDIR /app

# Accept build arg for API URL
ARG VITE_API_URL=http://localhost:5000
ENV VITE_API_URL=$VITE_API_URL

COPY package.json package-lock.json ./
RUN npm config set fetch-retries 5 && \
    npm config set fetch-retry-factor 10 && \
    npm config set fetch-retry-mintimeout 20000 && \
    npm config set fetch-retry-maxtimeout 3000000 && \
    npm config set fetch-timeout 600000 && \
    npm ci

COPY . .
RUN npm run build

FROM nginx:alpine AS production

COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]