# syntax=docker/dockerfile:1
# Build stage
FROM node:14.14.0-alpine as builder
ENV NODE_ENV production
WORKDIR /app
COPY ["package.json", "package-lock.json*", "./"]
RUN npm install --production
COPY . .
RUN npm run build

FROM nginx:1.21-alpine
EXPOSE 3000
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/build /usr/share/nginx/html