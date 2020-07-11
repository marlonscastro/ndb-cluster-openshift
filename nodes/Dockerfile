FROM ubuntu:18.04

RUN groupadd -r mysql && useradd -r -g mysql mysql

ENV MYSQL_ROOT_PASSWORD=16scm4

RUN apt update && apt upgrade -y && apt dist-upgrade -y && \
    apt install -y libclass-methodmaker-perl libssl1.1 libaio1 libmecab2 libnuma1 psmisc && \
    # Necessario para uso do systemctl - gerenciamento de serviços
    apt install -y systemd-container 
    # apt install -y debconf-utils gnupg gnupg1 gnupg2

VOLUME ["/usr/mysql-cluster"]
VOLUME ["/usr/local/mysql/data"]

COPY /pkg/* ./tmp/  

RUN { \
		echo mysql-cluster-community-server mysql-cluster-community-server/data-dir select ''; \
		echo mysql-cluster-community-server mysql-cluster-community-server/root_password password '16scm4'; \
		echo mysql-cluster-community-server mysql-cluster-community-server/root_password_again password '16scm4'; \
		echo mysql-cluster-community-server mysql-cluster-community-server/remove-test-db select false; \
	} | debconf-set-selections

# RUN echo "mysql-cluster-community-server mysql-cluster-community-server/root_password password root" | debconf-set-selections && \
#     echo "mysql-cluster-community-server mysql-cluster-community-server/root_password_again password root" | debconf-set-selections

RUN dpkg -i ./tmp/mysql-common_8.0.20-1ubuntu18.04_amd64.deb && \
    dpkg -i ./tmp/mysql-cluster-community-client-core_8.0.20-1ubuntu18.04_amd64.deb && \
    dpkg -i ./tmp/mysql-cluster-community-client_8.0.20-1ubuntu18.04_amd64.deb && \
    dpkg -i ./tmp/mysql-client_8.0.20-1ubuntu18.04_amd64.deb && \
    dpkg -i ./tmp/mysql-cluster-community-data-node_8.0.20-1ubuntu18.04_amd64.deb && \
    dpkg -i ./tmp/mysql-cluster-community-server-core_8.0.20-1ubuntu18.04_amd64.deb && \
    dpkg -i ./tmp/mysql-cluster-community-server_8.0.20-1ubuntu18.04_amd64.deb && \
    dpkg -i ./tmp/mysql-server_8.0.20-1ubuntu18.04_amd64.deb && \
    mkdir -p /etc/systemd/system


RUN rm -rf /var/lib/apt/lists/* \
	&& rm -rf /var/lib/mysql && mkdir -p /var/lib/mysql /var/run/mysqld \
	&& chown -R mysql:mysql /var/lib/mysql /var/run/mysqld /usr/local/mysql/data \
# ensure that /var/run/mysqld (used for socket and lock files) is writable regardless of the UID our mysqld instance ends up having at runtime
	&& chmod 777 /var/run/mysqld


# ENV MYSQL_MAJOR 8.0
# ENV MYSQL_VERSION 8.0.20-1ubuntu18.04

# Configurações do DATA-NODE
COPY /config/my.cnf ./etc/my.cnf

# Configurações do MYSQL Node
COPY /config/mysqld-my.cnf ./etc/mysql/my.cnf

# Configurações do Serviço
COPY /config/ndbd.service ./etc/systemd/system/ndbd.service

RUN systemctl enable ndbd
# RUN mysqld --initialize --user=root

# RUN systemctl daemon-reload
# RUN systemctl daemon-reexec
# RUN kill -TERM 1

# RUN mysql_secure_installation
# RUN mysql -uroot -p -e 'USE mysql; UPDATE `user` SET `Host`="%" WHERE `User`="root" AND `Host`="localhost"; DELETE FROM `user` WHERE `Host` != "%" AND `User`="root"; FLUSH PRIVILEGES;'
# ENTRYPOINT ["mysqld", "--initialize", "--user=root"]
ENTRYPOINT ["mysqld", "--initialize", "--user=root"]

CMD ["ndbd", "--nodaemon"] 


# docker run -it -d --net=cluster --name=mgmt-node --ip=192.168.0.2 msc/mgm-node
# docker run -it -d --net=cluster --name=node1 --ip=192.168.0.3 msc/data-node
# docker run -it -d --net=cluster --name=node2 --ip=192.168.0.4 msc/data-node
# r?zmW3Hr2Ogh

