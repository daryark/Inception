FROM alpine:3.20

ARG DB_NAME \
    DB_USER \
    DB_PASS

RUN apk update && apk add --no-cache mariadb mariadb-client

#tee - writes from in(from pipe here) to both: out and all the specified files
#sed - Stream EDitor (takes input from stream(if no file specified - from terminal, we take from: /etc/my.cnf.d/mariadb-server.cnf), -i modifies in-place instead of printing to stdout.
# s|regexp|replacement|[flags] - substitutes found regexp and format with flags, g - global (not only first occurance)
# mysql_install_db -utility to init the data dir for the db server by creating system db's and necessary data structs
#--datadir specifies, where to init the data dir, --user ensures the data dir is created under needed user
RUN mkdir /var/run/mysqld; \
    chmod 777 /var/run/mysqld; \
    { echo '[mysqld]'; \
      echo 'skip-host-cache'; \
      echo 'skip-name-resolve'; \
      echo 'bind-address=0.0.0.0'; \
    } | tee  /etc/my.cnf.d/docker.cnf; \
    sed -i "s|skip-networking|skip-networking=0|g" \
      /etc/my.cnf.d/mariadb-server.cnf

RUN mysql_install_db --user=mysql --datadir=/var/lib/mysql

EXPOSE 3306

#swithes user context to mysql (instead of root)
COPY requirements/mariadb/conf/create_db.sh .
RUN sh create_db.sh && rm create_db.sh
USER mysql
CMD ["/usr/bin/mysqld", "--skip-log-error"]
