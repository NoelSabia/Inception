FROM alpine:3.20

# ARG DB_NAME=${DB_NAME}
# ARG DB_USER=${DB_USER}
# ARG DB_PASSWORD=${DB_PASSWORD}
# ARG DB_ROOT_PASSWORD=${DB_ROOT_PASSWORD}

ARG DB_NAME=mysql
ARG DB_USER=nsabia
ARG DB_PASSWORD=password123
ARG DB_ROOT_PASSWORD=password123!

COPY conf/mariadb-server.cnf /etc/my.cnf 
COPY tools/create_user.sh ./

RUN apk update \
 && apk add mariadb mariadb-client mariadb-server-utils \
 && rm -rf /var/lib/apt/lists/*

RUN chmod +x ./create_user.sh

RUN sh ./create_user.sh

EXPOSE 3306

CMD ["mysqld"]
