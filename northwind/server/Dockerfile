# the environment: this is image will be based on Node v14
FROM node:14

## building the image
# environment variable to define my workspace
ENV APP_DIR=/app APP_PORT=3000 
ENV DB_USER=root DB_PASSWORD=changeit

# create and cd into APP_DIR
# mkdir /app
# cd/app
WORKDIR ${APP_DIR}

# copy the file from fortune into /app
# ADD local image (dot is here which is /app)
# copy the files
ADD main.js .
ADD package.json .
ADD package-lock.json .
# copy the directories
ADD config config
ADD public public
ADD views views

# COPY . ${APP_DIR} - check ignore files

# install the dependencies - package.json
RUN npm install

## running the image
# add healthcheck
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD curl --silent --fail http://localhost:${APP_PORT}/health || exit 1

# the  port the application is exposing
EXPOSE ${APP_PORT}

# run the application
ENTRYPOINT [ "node", "main.js" ]