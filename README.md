# Pgbouncer-guide
Installations and working steps for Pgbouncer
			Pg bouncer Installtion Guide
      
Step 1 - Installing pg bouncer using below commnads:
	sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
	  wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
	  apt update
	  apt install pgbouncer -y
    
Once pgbouncer is installed successfully ,it will automatically run.To check the the status use the below command
	sudo service pgbouncer status
  
Step 2 - Modify configuration files:
	In pgbouncer.ini file:
Define the postgres database name with its running port in database section.
		Eg. digitmarket = host=localhost port=5432 dbname=digitmarket
Change the unix_socket_dir to /tmp in pgbouncer section
		Eg. unix_socket_dir = /tmp
Add admin_users = <username> in pgbouncer section.
		Eg. admin_users = <username>
	In userlist.txt file:
Define the user with password in double quotes.
		Eg. "<username>" "<password>"
	In postgres pg_hba.conf file:
Change the Unix domain socket methods to md5 as in pgbouncer.ini file authetication.

Any changes in configuration files need to reload the pgbouncer service using below command
	systemctl reload pgbouncer.service
  
Step 3 - Connecting postgres with pgbouncer:
Connect the mentioned database in pgbouncer.ini file with user mentioned in userlist.txt file with below command
		psql -h localhost -U <username> -d digitmarket -p 6432
It ask for ur password,once its given it will open the database terminal.
To use the admin interface in pgbouncer execute the below commnad,
		 psql -h localhost -p 6432 -U <username> pgbouncer
	Here the pgbouncer give its all datas.
  
To start and stop pgbouncer service ,use the below commands
		systemctl start pgbouncer
		systemctl stop pgbouncer
    
Important commands in admin console:

SHOW STATS :
	It gives the total count of queries and its period time data with average of that using database.
  
SHOW STATS_TOTALS :
	Gives total count of stats of database.
  
SHOW STATS_AVERAGES :
	It gives the average values in stats
  
SHOW POOLS :
	It gives the active client connections and its wait time with pool  mode.
  
SHOW LISTS :
	It gives the total user clients with used and login statistics.It also shows the server details.
  
SHOW TOTALS :
	Gives all the statts of all the databases.
  
SHOW DATABASES :
	Shows details of databases connected with pgbouncer and its specifications provided.
  
SHOW CLIENTS :
	It gives total clients(users) in pgbouncer with their databases.Provides the IP and timing of their actions.
  
SHOW USERS :
	Gives the total users list.
  
SHOW FDS :
	FD means File descriptor.Its a internal command mechanism that checks UID to do online restart.
  
SHOW SOCKETS :
	Shows all the sockets of the pgbouncer(Includes clients and server details)
  
SHOW ACTIVE_SOCKETS :
	It gives the active socket information.
  
SHOW CONFIG :
	It gives the list of configurations of pgbouncer.
  
SHOW MEM :
	Shows the allocations of memory.
  
SHOW DNS_HOSTS :
	Shows the host details.
  
SHOW DNS_ZONES :
	Shows the zone wise details.
  
SHOW VERSION :
	Shows the pgbouncer version.
  
Important process controlling commands :

PAUSE  [db] :
	Pause the pgbouncer part.But already executed queries will run in background.
  
DISABLE db :
	Disable all new client connections to the database.
  
ENABLE db :
	Enable new client connections to the database.
  
RECONNECT [db] :
	Closes all open connections and reconnect with databases.
  
KILL db :
	Closes all client and server connections.
  
SUSPEND :
	Flushes all the buffers in sockets.
  
RESUME [db] :
	Start for kill,pause and suspend.
  
SHUTDOWN :
	Exit the processes from pgbouncer.
  
RELOAD :
	Reload the configuration files.
  
WAIT_CLOSE [db] :
	Wait untill all the server connections closed in case of configuration changes to apply for all databases or some switching concepts.
  
SET (configuration_name) :
	To change the configurations of pgbouncer.



