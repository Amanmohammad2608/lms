FROM node:16 AS builder
RUN mkdir /frontend
WORKDIR /frontend
COPY . /frontend
RUN npm install
RUN npm run build
FROM nginx
COPY --from=builder /frontend/build /usr/share/nginx/html
