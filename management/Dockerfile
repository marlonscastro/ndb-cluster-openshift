FROM ubuntu:18.04

# RUN groupadd -g 999 appuser && \
#     useradd -r -u 999 -g appuser appuser
# USER appuser

RUN apt update && apt upgrade -y && \
    apt install -y libclass-methodmaker-perl libssl1.1

VOLUME ["/usr/mysql-cluster"]

COPY mysql-cluster-community-management-server_8.0.20-1ubuntu18.04_amd64.deb ./tmp  

RUN dpkg -i ./tmp/*.deb && \
    # mkdir -p /usr/mysql-cluster/ && \
    mkdir -p /etc/systemd/system/

COPY config.ini ./usr/mysql-cluster/config.ini
COPY ndb_mgmd.service ./etc/systemd/system/ndb_mgmd.service

RUN sudo systemctl ndb_mgmd enable
RUN sudo systemctl daemon-reload

# RUN update-rc.d ndb_mgmd enable
# RUN sudo systemctl daemon-reload

CMD ["sudo", "ndb_mgmd", "--initial", "--config-file=/usr/mysql-cluster/config.ini"] 
# ndb_mgmd --initial --config-file=/usr/mysql-cluster/config.ini

# ENTRYPOINT ["read", "-s", "-n", "1", "-p", "Press any key to continue . . ."]


