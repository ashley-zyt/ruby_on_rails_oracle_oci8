# ruby_on_rails_oracle_oci8
In Ruby on rails, use Oracle database to install ruby-oci8 and configure datebase
ruby on rails oracle配置 oracle_enhanced，ruby-oci8安装 ，ORA-12154:TNS

ruby on rails oracle配置 oracle_enhanced，ruby-oci8安装 ，ORA-12154:TNS

1 Add corresponding gem
```
gem 'activerecord-oracle_enhanced-adapter', '~> 5.2', '>= 5.2.8'
gem 'ruby-oci8'
gem 'ruby-plsql', '~> 0.6.0'
```
activerecord-oracle_The enhanced adapter should be consistent with the rails version
Among them, ruby-oci8 installation is a little troublesome and needs to be configured.You need to download some Oracle installation packages before configuring them

2 Install ruby-oci8
 2.1 Download the SDK corresponding to Oracle and create a folder for storing oracle
      下载地址：https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html
      要下载的包是： instantclient-sqlplus-linux.x64-12.2.0.1.0.zip
      				instantclient-sdk-linux.x64-12.2.0.1.0.zip
      				instantclient-basiclite-linux.x64-12.2.0.1.0.zip
      例如：My storage location is /opt/oracle. Put the extracted files of sqlplus and SDK into the root directory of basic Lite
```
      		cd /opt/oracle
      		unzip instantclient-basiclite-linux.x64-12.2.0.1.0.zip
      		unzip instantclient-sdk-linux.x64-12.2.0.1.0.zip
      		cd instantclient-basiclite-linux.x64-12.2.0.1.0/instantclient_12_2
      		sudo ln -s libclntsh.so.12.1 libclntsh.so  
```
 2.2 Create Oracle instant client system variable
```
  	export LD_LIBRARY_PATH=/opt/oracle/instantclient-basiclite-linux.x64-12.2.0.1.0/instantclient_12_2
```
 2.3 Installruby-oci8
```
    sudo env LD_LIBRARY_PATH=/opt/oracle/instantclient-basiclite-linux.x64-12.2.0.1.0/instantclient_12_2 /usr/bin/gem install ruby-oci8
```
3 config/datebase.yml
```
	development:
	  <<: *default
	  adapter: oracle_enhanced
	  encoding: unicode
	  port: 1521
	  database: HELOWIN
	  username: username
	  password: password
	  pool: 5
	  variables:
	    statement_timeout: 5000
```
	It is possible to prompt an error after the project is started
	ORA-12154:TNS:could not resolve the connect identifier specified，则另外需要配置tnsnames.ora
	In order to connect with the server, the client must first contact the listening process on the server. Oracle describes the connection information through the connection descriptor in the tnsnames.ora file。tnsnames.ora Is built on the client. If it is a client / server structure and only one machine on the whole network has an Oracle database server installed, you only need to define the file on each client that wants to access the Oracle server, and there is no need to define the file on the server
	tnsnames.ora the storage path is/etc/tnsnames.ora ,The file content schema is：
	``
	LISTENER_HELOWIN =
	  (ADDRESS = (PROTOCOL = TCP)(HOST = xx.xx.xx.xx)(PORT = 1521))
	HELOWIN =
	  (DESCRIPTION =
	    (ADDRESS = (PROTOCOL = TCP)(HOST = xx.xx.xx.xx)(PORT = 1521))
	    (CONNECT_DATA =
	      (SERVER = DEDICATED)
	      (SERVICE_NAME = service_name)
	    )
	  )
	``
4. Next, start the project normally