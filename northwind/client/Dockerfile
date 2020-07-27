FROM node:14 as builder

ENV APP_DIR=/app APP_PORT=80

WORKDIR ${APP_DIR}
ADD package.json .
ADD package-lock.json .

RUN npm install

COPY . ./
RUN npm run build --prod
# build folder dist/client

FROM nginx:latest

COPY --from=builder /app/dist/client /usr/share/nginx/html
COPY docker/nginx/default.conf /etc/nginx/conf.d/default.conf