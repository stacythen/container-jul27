# get mysql image
FROM mysql:5.7
#FROM mysql@sha256:bf17a7109057494c45fba5aab7fc805ca00ac1eef638dfdd42b38d5a7190c9bb

ENV MYSQL_ROOT_PASSWORD=changeit MYSQL_PORT=3306 MYSQL_DATABASE=northwind MYSQL_DB_PATH=/var/lib/mysql

# copy the SQL files to docker-entrypoint-initdb.d for seeding
ADD 0-northwind-default-current-timestamp.sql /docker-entrypoint-initdb.d
ADD 1-northwind-data.sql /docker-entrypoint-initdb.d

# add healthcheck
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD mysqladmin ping -u root -p ${MYSQL_ROOT_PASSWORD} || exit 1

EXPOSE ${MYSQL_PORT}

# check docker hub mysql volume path
# volume
VOLUME ${MYSQL_DB_PATH}