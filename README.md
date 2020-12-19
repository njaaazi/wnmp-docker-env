# wnmp-docker-env

## Table of Contents

- [Setup and configuration (Windows)](#setup_windows) - environment and configuration setup options
    - [Requirements](#requirements_windows) - Things you need to have
        - [Windows](#windows_windows) - Windows 10 Pro
        - [Docker](#docker_windows) - Docker

    - [Create Directory](#create_dir_windows) - Create these directories 
    - [.env_example](#env_windows) - environment variable declaration for docker-compose
    - [Hosts](#hosts_windows) - Change hosts file to your domain

- [Starting project (Windows)](#start_windows) - Start your project using dokcer
    - [Powershell](#powershell_windows) - Open ```Windows powershell```
    - [Start project](#start_project_windows) - Start your project

- [Database (Windows)](#database_windows) - Managing database
    - [phpmyadmin](#phpmyadmin_windows) - Open phpmyadmin in browser
    - [mariadb](#mariadb_windows) - Connect to mariadb 
    - [Import db with cli](#importdb_windows) - Importing your db with mariadb cli
    
- [Issues](#issues_windows) - Common issues
    - [MySQL server has gone away](#mysql_has_gone_away_windows) - ERROR 2006 (HY000): MySQL server has gone away while trying to import a db
 
## <a name="setup_windows"></a>Setup and configuration (Windows)
### <a name="requirements_windows"></a>Requirements
#### <a name="windows_windows"></a>Windows 10 Pro
To install docker you’ll need Windows 10 Pro. Make sure that you have Virtualization (VT) enabled on your Windows otherwise you can't install docker.
#### <a name="docker_windows"></a>Docker
 Download <a href="https://docs.docker.com/docker-for-windows/install/"> Docker for Windows </a>  
 **Note:** Run as administrator after you install on your machine.


### <a name="create_dir_windows"></a>Create directory  
We need these directories before we start the project in docker.  
To create these directories on Windows please use ```Command Prompt (CMD)```
```
md db logs\nginx mysql wordpress
```

### <a name="env_windows"></a>.env
`.env_example` file has been included to set docker-compose variables without having to modify the docker-compose.yml file itself.
Create a file named `.env` from the `.env_example`
```
cp .env_example .env
```
or
```
copy .env_example .env
```

Most important variables that need to be modifed by your site needs are:

***SITE_NAME***
Your specific project that you are working on i.e sportal or wsn 
```
#site name
SITE_NAME=example
```

***WORDPRESS_DB_NAME***  
Where needs to be the same as MYSQL_DATABASE variable.  
**Note:** This is where it's specified in ```wp-config.php``` so it matches you mysql database name. (It does automatically though, you don't need to change anything on ```wp-config.php``` file).
```
WORDPRESS_DB_NAME=database
```

*MYSQL_DATABASE*   
Where it will create the database based on this name.
```
MYSQL_DATABASE=database
```

***DATABASE_FILE_NAME***   
**If you want a to install wordpress on a new fresh database, leave it as it is, it doesn't matter in this case.**   
This is optional because it should be used if you want to import a database from a site, so it's quite important in this case.   
You should add the name of your database file that you need to import.
```
# db file name to import (optional)
DATABASE_FILE_NAME=database
```

### <a name="hosts_windows"></a>Hosts File
Change your hosts file to your domain i.e wp.local (Which is by default in new project)
Go to this directory:
```
C:\Windows\System32\drivers\etc
```

Edit ```hosts``` file:
```
127.0.0.1 wp.local
```

**Important** If you change this domain name to something else in your needs, please make sure to change it also in the ```nginx\default.conf``` and there change it the server name to the new domain ``` server_name new.domain; ```  

## <a name="start_windows"></a>Start your project (Windows)
### <a name="powershell_windows"></a>Open Windows powershell 
You need to open ```Windows powershell``` and  **Run as administrator**.    
CD to your project folder that you've cloned, i.e ```cd C:\Users\YourUser\Desktop\wnmp-dcoker-env```  

### <a name="start_project_windows"></a>Starting your project
**Note:** Make sure you have Docker up and running on your machine.  
Run this command:
```
docker-compose up -d
```

**Note:** Sometimes ```docker-compose up -d``` won't work in the first try with ```Windows Powershell```, so please try again the command if something fails.

Run this command to see all containers that were created:
```
docker-compose ps -a
```

Go to your browser with the domain that you have set on your hosts file, in our example is ```wp.local```.

**Note:** If you wan't to import another db into your site, you don't have to proceed on installing wordpress in this stage as later you'd have to remove the db first and then import the database, instead go to Managing Database below and import your db.


### <a name="database_windows"></a> Managing Database
We can manage our database in different ways with ```phpmyadmin```, ```mariadb```,```other db client```.  
### <a name="phpmyadmin_windows"></a>Open phpmyadmin in browser
To open phpmyadmin in your browser you can simply go in this address:  
```
http://localhost:8082/
```

### <a name="mariadb_windows"></a>Connect to mariadb
To connect to mariadb you'll have to go into Docker dashboard again and click ```cli``` button:

After you open ```cli``` you can use it to connect into your database:
```
mysql -uroot -p
```

### <a name="importdb_windows"></a>Importing your db with mariadb cli
To import we first we need to make sure that the database in our ```db``` directory was imported inside container, to do that we can simply connect to ```mysql/mariadb cli``` and check if your ```database.sql``` is inside ```docker-entrypoint-initdb.d``` directory.  
After you are connected to mysql cli, go to docker-entrypoint-initdb.d  ```cd docker-entrypoint-initdb.d``` and list items inside that with this command ```ls```, if your specific sql file is there then you can continue on importing it using this command:

Considering that you are in the ```docker-entrypoint-initdb.d``` directory.
```
mysql -uroot -ppassword database < ./database.sql
```


### <a name="issues_windows"></a> Common issues
Check for the list below for the common issues that might happen
### <a name="mysql_has_gone_away_windows"></a>ERROR 2006 (HY000): MySQL server has gone away while trying to import a db
If you are having this issue while importing your db please check this link:
https://stackoverflow.com/questions/10474922/error-2006-hy000-mysql-server-has-gone-away  

In order to modify you'll need a text editor you can install ```nano``` on your mysql cli (shell) as you'd do in linux:  
```
apt-get update
apt-get install nano
```
You should modify ```my.cnf``` file on your mysql, to do that cd to this directory ```/etc/mysql``` you can find this file and edit it as requested.  
