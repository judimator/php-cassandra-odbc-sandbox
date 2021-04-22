FROM php:8.0

ENV CASSANDRA_PATH="/var/cassandra"
ENV CASSANDRA_DRIVER_PATH="$CASSANDRA_PATH/driver/odbc"

COPY cassandra /var/cassandra

RUN cd /var/cassandra/manager \
    && gzip -d unixODBC-2.3.9.tar.gz \
    && tar -xvf unixODBC-2.3.9.tar \
    && unixODBC-2.3.9/configure --prefix=/usr/local/unixODBC --enable-gui=no \
    && make \
    && make install \
    && rm unixODBC-2.3.9.tar

RUN set -x \
    && docker-php-source extract \
    && cd /usr/src/php/ext/odbc \
    && phpize \
    && sed -ri 's@^ *test +"\$PHP_.*" *= *"no" *&& *PHP_.*=yes *$@#&@g' configure \
    && ./configure --with-unixODBC=shared,/usr/local/unixODBC \
    && docker-php-ext-install odbc \
    && docker-php-source delete

ENV ODBCINI=/var/cassandra/driver/odbc/lib/odbc.ini
ENV ODBCSYSINI=/var/cassandra/driver/odbc/lib
ENV DATASTAXCASSANDRAODBC=/var/cassandra/driver/odbc/lib/simba.cassandraodbc.ini
ENV LD_LIBRARY_PATH=/usr/local/unixODBC/lib
