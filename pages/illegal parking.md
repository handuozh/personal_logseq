- Demonstration steps [[Jun 28th, 2021]]
	- Prerequisites
		- username: `mpe`
		- password: `Gzwx2021`
		- working dir: `/home/mpe/`
	-
	  1. docker load
		- `docker load < illegal_detect.tar.gz`
	-
	  2. data preparation
		- 假设宿主机上的图片文件夹在`$HOME/datatest/`里面
		- 将URA提供的两个任务的图片文件夹（354张、87张）放到`datatest`中，分别命名为`lpr`和`detect`
	-
	  3. docker run
		-
		  ```shell
		  docker run --gpus all -it -v $HOME/datatest:/home/mpe/test illegal_detect:1.12
		  ```
	-
	  4. Decryption
		-
		  ```shell
		  sudo dd if=tar_py.des3 | openssl des3 -d -k Gzwx2021 | tar zxf - && sudo cp tar/* . -r
		  ```
	-
	  5. Car/lane detection
		-
		  ```shell
		  sudo python3 detect.py --source ./test/detect
		  ```
	-
	  6. License plate recognition (LPR)
		- modify data folder name of `input_dir` by [[vim]]
			- `sudo vim lp.sh`
			-
			  ```shell
			  input_dir='test/lpr'
			  ```
		-
		  ```shell
		  sudo ./lp.sh
		  ```
	-
	  7. Copy out the results to host machine
		-
		  ```shell
		  sudo cp -r runs/detect/exp test/detect_result
		  sudo cp -r lpr/outputs test/lpr_result
		  ```
	-