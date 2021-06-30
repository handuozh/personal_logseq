- #related [[file decryption]]
- Suggested encryption method
	-
	  ```shell
	  openssl enc -aes-256-cbc -md sha512 -pbkdf2 -iter 100000 -salt -in InputFilePath -out OutputFilePath
	  ```
		- `-aes-256-cbc` is more secure while `des3` has been deprecated in 2017
		- `-md sha512` is faster compared to `SHA-256` and more secure
		- `-pbkdf2` for key derivation function 2 algorithm
	- To decode
		-
		  ```shell
		  openssl enc -aes-256-cbc -md sha512 -pbkdf2 -iter 1000 -salt -d -in InputFilePath
		  ```
			- add `-d` to decode
- https://www.openssl.org/docs/manmaster/man1/genrsa.html
- https://www.openssl.org/docs/manmaster/man1/openssl.html
	- Encryption, Decryption, and Encoding Commands
		- `base64`
		- des3, desx, des-ede3, des-ede3-cbc, des-ede3-cfb, des-ede3-ofb
			- Triple-DES Cipher
		- aes256, aes-256-cbc
			- AES-256 Cipher
			- maximum protection or the 128-bit version