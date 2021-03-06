![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Zabbix_3.2/src/img/zabbix.jpg)

##                                      Tutorial de instalação Raiz do Zabbix Server 3.4 no CentOs 7 (x64) com banco de dados MariaBD!


## Introdução.

Zabbix é um software que monitora diversos parâmetros de uma rede como a integridade e desempenho dos servidores. Oferece excelentes relatórios e visualização de dados de recursos com base nos dados armazenados, e usa um mecanismo de notificação flexível que permite aos usuários configurar e-mail com alertas para qualquer evento, o que permite uma reação rápida para os problemas do servidor.
Zabbix e Open Source e multiplataforma, livre de custos de licenciamento, sendo utilizada para monitorar a disponibilidade e o desempenho de aplicações, ativos e serviços de rede por todo o mundo.

Na lista abaixo temos algumas vantagens de se utilizar o Zabbix:

* Solução Open Source;
* Suporte para SNMP (v1, v2 e v3);
* Monitoramento distribuído com administração centralizada na web;
* Agentes de alta performance (software de cliente para Linux, Solaris, HP-UX, AIX, FreeBSD, OpenBSD, OS X, Tru64/OSF1, , Windows Server Windows Desktop);
* Permissões flexíveis de usuário;
* Interface baseada na web.


## Requisitos:

Servidor CentOs 7,Apache2, MariaDB.


## Instalação.

##
###### 1) Vamos acessar o servidor via ssh ou interface grafica e atualizar o repositório como root:

```sh
# yum update && yum upgrade
```

![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz01.PNG)
##

###### 1.1) Execute o comando abaixo para instalar as dependencias:
```sh
# yum -y install php-cli php-common php-devel php-pear php-gd php-mbstring php-mysql php-xml vim php7.0-bcmath php7.0-mbstring php-sabre-xml mariadb-server  mariadb && systemctl start mariadb
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz01-1.PNG)
##

###### 2)  Para a instalação do Zabbix Server 3.4 é necessário incluir no repositório as informações atualizadas do Zabbix:

```sh
# cd /tmp

# rpm -ivh https://raw.githubusercontent.com/MagnoMonteCerqueira/Zabbix/master/Dicas_e_Truques/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/Arquivos/zabbix-release-3.4-1.el7.centos.noarch.rpm
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz02.PNG)
##

###### 2.2) Vamos atualizar o repositorio e Instalar o Zabbix Server 3.4:
```sh
# yum update && yum install zabbix-server-mysql zabbix-frontend-php zabbix-agent zabbix-web-mysql -y 
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz03.PNG)
##

###### 3)  Após todos os passos anteriores, vamos acessar a mariadb e criar banco de dados e usuario para utilização do Zabbix Server, mais antes vamos instalar o banco de dados:

```sh
# yum install mariadb-server
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz-mariadb-01.PNG)

##
```sh
# systemctl enable mariadb
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz-mariadb-02.PNG)
##

iniciando o banco de dados
```sh
#  systemctl start mariadb
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz-mariadb-03.PNG)
##

Vamos Acessar o banco de dados:
```sh
#  mysql -u root
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz-mariadb-04.PNG)
##

###### 4)  Apos banco de dados instalado vamos preparar o banco de dados para o Zabbix Server:

Criar o banco de dados para o Zabbix:
```sh
# create database zabbix character set utf8 collate utf8_bin;
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz-mariadb-05.PNG)

##
Criar usuairo para o Zabbix Server e seu privilegio de acesso:
```sh
# grant all privileges on zabbix.* to zabbix@localhost identified by 'SENHA-USUARIO-ZABBIX';
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz-mariadb-06.PNG)

##
Vamos sair do banco de dados:
```sh
# quit;
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz-mariadb-07.PNG)

##
Importando as tabelas para uso do zbabix server para o banco de dados criado:
```sh
# zcat /usr/share/doc/zabbix-server-mysql-3.4.3/create.sql.gz | mysql -uzabbix -p zabbix
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz-mariadb-08.PNG)

OBS: Digite a senha do usuario zabbix criado anteriormente para importar as tabelas.


##
###### 5)  Vamos editar o arquivo de configuração do Zabbix Server e inserir as informações abaixo:

```sh
# vim /etc/zabbix/zabbix_server.conf
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz05.PNG)


##
```sh
#...
DBHost=localhost
#...
DBName=zabbix
#...
DBUser=zabbix
#...
DBPassword=SENHA-USUARIO-ZABBIX
#...
```

![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz06.PNG)


##
###### 6)  Vamos editar o arquivo de configuração do Zabbix Server dentro do Apache e inserir as informações abaixo:

```sh
# vim /etc/httpd/conf.d/zabbix.conf
```
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Dicas_e_Truques/src/img/Zabbix_Server/Instalacao/3.4/CentOs/Raiz/centos-raiz07.PNG)


##

Na Linha 19 e 28 remova o comentario (#) e coloque a data e hora de sua regiao.

```sh
#  php_value date.timezone Europe/Riga
```
##

No meu caso:
```sh
#  php_value date.timezone America/Sao_Paulo
```
##



###### 7)  Vamos Reiniciar o servidor Apache e iniciar os servicos do Zabbix Server e Zabbix Agent:
##
Apache:
```sh
# /etc/init.d/apache2 restart
```
##
Zabbix Server
```sh
# systemctl enable zabbix-server
```
##
Zabbix Agent
```sh
# systemctl enable zabbix-agent
```

###### 8)  A interface web do Zabbix estará disponível em http://SEU-IP/zabbix através do seu navegador, vamos configurar o Zabbix Server via web:


###### 8.1) Acessando via web o IP do Zabbix Server, clique em Next step.
##
Clique em Next step:
##
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Zabbix_3.4/src/img/Zabbix_server/instalacao01.PNG)
##
Clique em Next step:
##
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Zabbix_3.4/src/img/Zabbix_server/instalacao02.PNG)
##
Insira a senha criada para usuario Zabbix, Clique em Next step:
##
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Zabbix_3.4/src/img/Zabbix_server/instalacao03.PNG)
##
Em Name Coloque o nome que desejar, Clique em Next step:
##
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Zabbix_3.4/src/img/Zabbix_server/instalacao04.PNG)
##
Em Pre-Instalação veja o resumo das configurações, Clique em Next step:
##
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Zabbix_3.4/src/img/Zabbix_server/instalacao05.PNG)
##
Em Install, Clique em Next step:
##
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Zabbix_3.4/src/img/Zabbix_server/instalacao06.PNG)
##
Na tela de username e password (tela inicial do Zabbix Server) coloque no primeiro acesso o usuário e senha padrões são: Admin/zabbix:
##
![Alt Text](https://github.com/MagnoMonteCerqueira/Zabbix/blob/master/Zabbix_3.4/src/img/Zabbix_server/instalacao07.PNG)
##



##

## Contatos:


* Telegram: @Magnopeem
* Skype: magnopeem_rj@hotmail.com
* E-mail: magnopeem@gmail.com
* Linkedin: https://br.linkedin.com/in/magno-monte-cerqueira-976b1587
