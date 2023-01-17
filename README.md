# Install Kamailio in Ubuntu 11.6

1. apt search kam

2. apt install kamailio kamailio-mysql-modules -y

3. apt install  mariadb-server -y

4. systemctl enable mariadb

5. systemctl start mariadb 

6. Entry kamctlrc 
	nano /etc/kamailio/kamctlrc 
	
7. Uncomment the DBENGINE parameter by removing the pound symbol and make sure the value equals MYSQL. The parameter should look like this afterwards: 
	DBENGINE=MYSQL
	nano /etc/kamailio/kamctlrc
	
8. Uncomment and setup the Database Read/Write and Database Read/Only fields. You can just use the default values for right now and you can change them at a later time.

	## database read/write user
	DBRWUSER="kamailio"

	## password for database read/write user
	DBRWPW="kamailiorw"

	## database read only user
	DBROUSER="kamailioro"

	## password for database read only user
	DBROPW="kamailioro"
	
9. Create the Kamailio Database Schema
	/usr/sbin/kamdbctl create
	
10. Yes in all de question
	
11. Entry in kamailio.cfg
	vi /etc/kamailio/kamailio.cfg
	
12. Add the following after #!KAMAILIO
	#!define WITH_MYSQL
	#!define WITH_AUTH
	
13. Update the DBURL line to match the username and password you set in /etc/kamailio/kamctlrc earlier The line looks like this by default:
	#!trydef DBURL "mysql://user:password@localhost/kamailio"
	
	Ex: #!trydef DBURL "mysql://kamailio:kamailiorw@localhost/kamailio"
	
14. Start the Kamailio Server
	service start kamailio
	
## Test Kamailio

15. Create SIP User Accounts
	kamctl add 1001@dopensource.com opensourceisneat
	
16. configure the 3cx to exten
	user: 1001
	pass: dopensource.com
	IP: <ip the serve>
	
17. Check register
	kamctl ul show
	

Link the tutorial original: https://dopensource.com/2019/05/15/kamailio-v52-debian-quick-install/