#base image multi 
FROM node:24.22-bookworm-slim@sha256:01d566e8d94d8c619591d78652babfccd3c6992a3dffa9ed5ab39e11cf6c70e9 AS builder

WORKDIR /app

COPY package.json package-lock.json ./ 

COPY . .

ENV NODE_OPTIONS=--max-old-space-size=6144

RUN npm ci

RUN npm run build

#nginx runner used to decrease CVEs from the node pks dependencies
FROM nginx:alpine@sha256:713fc2ebfd92e8217db451477182ee91944b123d2b1ee9fd5a8c6e91ca6c3a11 AS runner

RUN apk update && apk upgrade --no-cache

COPY --from=builder /app/build/prod /usr/share/nginx/html

COPY nginx.conf etc/nginx/conf.d/default.conf

EXPOSE 80


