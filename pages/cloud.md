---
title: cloud
---

- AWS
	- EC2
- Services
	- MySQL
		- https://docs.gitea.io/en-us/database-prep/
			- `mysql -u root -p`
			-
			  ```shell
			  SET old_passwords=0;
			  CREATE USER 'gitea' IDENTIFIED BY 'gitea';
			  ```
			- Create database with ^^UTF-8^^ charset and collation
				-
				  ```shell
				  CREATE DATABASE giteadb CHARACTER SET 'utf8mb4' COLLATE 'utf8mb4_unicode_ci';
				  ```
			- Grant all privileges on the database to database user created above
				-
				  ```shell
				  GRANT ALL PRIVILEGES ON giteadb.* TO 'gitea';
				  FLUSH PRIVILEGES;
				  ```
			- exit the console
			- test connection
				-
				  ```shell
				  mysql -u gitea -h 203.0.113.3 -p giteadb
				  ```
					- where `gitea` database username
					- `giteadb` database name
					- for local database omit `-h` option
				-
	- [[gitea]]
		-