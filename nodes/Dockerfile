FROM ubuntu:18.04

# RUN groupadd -g 999 appuser && \
#     useradd -r -u 999 -g appuser appuser
# USER appuser
ENV MYSQL_ROOT_PASSWORD=16scm4

RUN apt update && apt upgrade -y && apt dist-upgrade -y && \
    apt install -y libclass-methodmaker-perl libssl1.1 libaio1 libmecab2 libnuma1 psmisc && \
    # Necessario para uso do systemctl - gerenciamento de serviços
    apt install -y systemd-container

VOLUME ["/usr/mysql-cluster"]
VOLUME ["/usr/local/mysql/data"]

COPY /pkg/* ./tmp/  

RUN dpkg -i ./tmp/*.deb && \
    mkdir -p /etc/systemd/system/



# ENV MYSQL_MAJOR 8.0
# ENV MYSQL_VERSION 8.0.20-1ubuntu18.04

# RUN echo "deb http://repo.mysql.com/apt/ubuntu/ bionic mysql-cluster-${MYSQL_MAJOR}" > /etc/apt/sources.list.d/mysql.list

# RUN { \
# 		echo mysql-cluster-community-server mysql-cluster-community-server/data-dir select ''; \
# 		echo mysql-cluster-community-server mysql-cluster-community-server/root-pass password ''; \
# 		echo mysql-cluster-community-server mysql-cluster-community-server/re-root-pass password ''; \
# 		echo mysql-cluster-community-server mysql-cluster-community-server/remove-test-db select false; \
# 	} | debconf-set-selections \
# 	&& apt-get update && apt-get install -y mysql-cluster-community-client="${MYSQL_VERSION}" mysql-cluster-community-server-core="${MYSQL_VERSION}" && rm -rf /var/lib/apt/lists/* \
# 	&& rm -rf /var/lib/mysql && mkdir -p /var/lib/mysql /var/run/mysqld \
# 	&& chown -R mysql:mysql /var/lib/mysql /var/run/mysqld \
# 	&& chmod 777 /var/run/mysqld




# Configurações do DATA-NODE
COPY /config/my.cnf ./etc/my.cnf
# Configurações do API-MYSQL Node
COPY /config/mysqld-my.cnf ./etc/mysql/my.cnf

# Configurações do Serviço
COPY /config/ndbd.service ./etc/systemd/system/ndbd.service

RUN systemctl enable ndbd
    
# RUN systemctl daemon-reload
# RUN systemctl daemon-reexec
# RUN kill -TERM 1

CMD ["ndbd", "--nodaemon"] 

# docker run -it -d --net=cluster --name=mgmt-node --ip=192.168.0.2 msc/mgm-node
# docker run -it -d --net=cluster --name=node1 --ip=192.168.0.3 msc/data-node
# docker run -it -d --net=cluster --name=node2 --ip=192.168.0.4 msc/data-node

