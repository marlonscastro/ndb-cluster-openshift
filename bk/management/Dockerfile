FROM ubuntu:18.04

# RUN groupadd -g 999 appuser && \
#     useradd -r -u 999 -g appuser appuser
# USER appuser

RUN apt update && apt upgrade -y && apt dist-upgrade -y && \
    apt install -y libssl1.1 && \
    # Necessario para uso do systemctl - gerenciamento de serviços
    apt install -y systemd-container

VOLUME ["/usr/mysql-cluster"]

COPY /pkg/* ./tmp/  

RUN dpkg -i ./tmp/*.deb && \
    # mkdir -p /usr/mysql-cluster/ && \
    mkdir -p /etc/systemd/system/

# Configurações do Cluster Gerente
COPY /config/config.ini ./usr/mysql-cluster/config.ini
# Configurações do Serviço
COPY /config/ndb_mgmd.service ./etc/systemd/system/ndb_mgmd.service

RUN systemctl enable ndb_mgmd
# RUN systemctl daemon-reload

# RUN systemctl daemon-reexec
# RUN kill -TERM 1

# RUN ndb_mgmd --initial --config-file=usr/mysql-cluster/config.ini --ndb-nodeid=1

CMD ["ndb_mgmd", "--initial", "--config-file=usr/mysql-cluster/config.ini", "--ndb-nodeid=1", "--nodaemon"]

# ndb_mgmd --initial --config-file=/usr/mysql-cluster/config.ini

# CMD ["read", "-s", "-n", "1", "-p", "'NDB MGMT Running...'"]
# CMD ["sleep", "10m"]

# docker run -it -d --net=cluster --name=mgmt-node --ip=192.168.0.2 msc/mgm-node
# docker run -it -d --net=cluster --name=node1 --ip=192.168.0.3 msc/data-node

