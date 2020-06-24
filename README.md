# ndb-cluster-openshift

ubuntu 18

sudo apt update
sudo apt dist-upgrade 


########## DATA-NODES ########################################################

dependencias
* libclass-methodmaker-perl

data dir
	/usr/local/mysql/data
config files
	/etc/my.cnf
to do 
	start up services

########## MYSQL-NODES ########################################################

conjuntos de pacotes .deb já baixados

* libaio1 libmecab2
config files 
	/etc/mysql/my.cnf


########## NDB-MANAGEMENT ########################################################

data dir
	/usr/mysql-cluster
config files 
	config.ini
todo 
	start up services 

passo a passo 

criar -> mkdir -p /usr/mysql-cluster
e abre /usr/mysql-cluster/config.ini

[ndbd default]
NoOfReplicas=2

[ndbd_mgmd]
hostname=*******ip do server*******
datadir=/usr/mysql-cluster
NodeId=1

[ndbd]
hostname=*********ip do Nó 1*********
NodeId=11
datadir=/usr/local/mysql/data

[ndbd]
hostname=*********ip do Nó 2*********
NodeId=12
datadir=/usr/local/mysql/data

[mysqld]
hostname=*********ip do Nó 1*********
NodeId=21

[mysqld]
hostname=*********ip do Nó 2*********
NodeId=22

e configura o serviço 

sudo vim /etc/systemd/system/ndb_mgmd.service com o conteudo abaixo 

[Unit]
Description=MySQL NDB Cluster Management Server
After=network.target audit.service

[Service]
Type=forking
ExecStart=/usr/sbin/ndb_mgmd -f /usr/mysql-cluster/config.ini
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure

[Install]
WantedBy=multi-user.target

e depois executa 
	"sudo systemctl enable ndb_mgmd"
	"sudo systemctl daemon-reload"

executa o management 
sudo ndb_mgmd --initial --config-file=/usr/mysql-cluster/config.ini

depois, se necessário 
sudo pkill ndb_mgmd

ao executar em um dos nós 
ndb_mgm -e show 
	o sistema mostra que os nós ainda não estão conectados; 

ao executar nos nós o comando "ndbd" os data-nodes conectarão ao server.

para conectar os mysql-api necessita "sudo service mysql restart"

########## COMANDOS UTEIS ########################################################
	nbd_mgm -e show -> List all clusters 
	ndb -> start the node
	pkill ndbd -> End process nodes 
	pkill ndb_mgmd -> Finaliza do Management
	

.................................. configuração ........................................

Data nodes oooooooooooooooooooooooooooooooo

	/etc/my.cnf

[mysql_cluster]
ndb-connectstring=******ip do server********

diretorios 
mkdir -p /usr/local/mysql/data

Configurar os serviços #########################3
sudo vim /etc/systemd/system/ndbd.service

cria o arquivo acima e coloca o conteúdo abaixo 
e depois executa 
	"sudo systemctl enable ndbd"
	"sudo systemctl daemon-reload"

[Unit]
Description=MySQL NDB Data Node Daemon
After=network.target audit.service

[Service]
Type=forking
ExecStart=/usr/sbin/ndbd
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure

[Install]
WantedBy=multi-user.target

Mysql API oooooooooooooooooooooooooooooooo

	/etc/mysql/my.cnf

[mysqld]
ndbcluster
ndb-connectstring=******ip do server********

[mysql_cluster]
ndb-connectstring=******ip do server********

----------------------------------------------------

Comandos uteis ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

sudo ndb
sudo pkill ndb
sudo ndb_mgm -e show 



