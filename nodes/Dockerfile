FROM ubuntu:18.04

# RUN groupadd -g 999 appuser && \
#     useradd -r -u 999 -g appuser appuser
# USER appuser

RUN apt update && apt upgrade -y && apt dist-upgrade -y && \
    apt install -y libclass-methodmaker-perl libssl1.1 libaio1 libmecab2 libnuma1 psmisc && \
    # Necessario para uso do systemctl - gerenciamento de serviços
    apt install -y systemd-container

VOLUME ["/usr/mysql-cluster"]

COPY /pkg/* ./tmp/  

RUN dpkg -i ./tmp/*.deb && \
    # mkdir -p /usr/mysql-cluster/ && \
    mkdir -p /etc/systemd/system/

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

CMD ["ndbd"] 


# docker run -it -d --net=cluster --name=node1 --ip=192.168.0.3 msc/data-node

