---
title: ssh
---

- Tools
	- Linux
	- Windows
		- cmder
		- xshell
		- mobaxterm
		- wsl and windows terminal
- Configuration of SSH keys
	- Linux
		- Generate RSA keys
			-
			  ```bash
			  			  ssh-keygen -t ed25519 -C "your_email@example.com"
			  			  ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
			  			  ```
		- Check existing SSH keys
			- whether under `~/.ssh`
			- ensure `ssh-agent` is running
				-
				  ```
				  				  eval `ssh-agent -s`
				  				  `````
			- Add SSH private key to the ssh-agent
				-
				  ```
				  				  ssh-add ~/.ssh/id_ed25519
				  				  ```
		- Copy the **public** key to the server
		  		  ```
		  		  ssh-copy-id -i ~/.ssh/public-key-ecdsa user@host
		  		  ```
	- Windows
		- Replace `ssh-copy-id`
			-
			  ```
			  ## ssh user@remote -p port 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
			  type C:\Users\mindp\.ssh\id_rsa.pub | ssh mind04@192.168.1.128 -p 7080 "cat >> .ssh/authorized_keys"
			  ```
	- Set the alias
		- add the following to`~/.ssh/config`
			-
			  ```
			  			  Host lab
			  			   HostName remote_addr
			  			   User user_name
			  			   Port port_no
			  			  ```
			- Then can directly connect to`ssh lab`
- Transfer files
	- Local to remote
		-
		  ```
		  		  scp -P port /path/to/local/file user@remote:/path/to/remote/file
		  		  ```
	- Download to local site from remote
		-
		  ```
		  		  scp lab:/path/to/remote/file /path/to/local/file
		  		  ```
	- `-r` for folders