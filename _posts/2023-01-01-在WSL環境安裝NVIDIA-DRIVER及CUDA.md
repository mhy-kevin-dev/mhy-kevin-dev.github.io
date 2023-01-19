
### 不要在WSL內直接下指令安裝nvidia driver

1. 首先要在windows安裝driver, 到官網 [https://www.nvidia.com/Download/index.aspx](https://www.nvidia.com/Download/index.aspx) 選擇對應的版本來安裝
2. 安裝完成後，在windows的cmd下指令`nvidia-smi`，如果正常顯示GPU狀態，表示安裝完成
3. 到官網的CUDA [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)  查詢wsl2用的安裝指令, 記得版本要選Ubuntu-wsl的
4. 開啟WSL，在裏面貼步驟三的指令一一安裝

![步驟3-圖1](https://i.imgur.com/dn5BQNi.png)
![步驟3-圖2](https://i.imgur.com/3RBN3s1.png)

```sh
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/12.0.0/local_installers/cuda-repo-wsl-ubuntu-12-0-local_12.0.0-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-12-0-local_12.0.0-1_amd64.deb
sudo cp /var/cuda-repo-wsl-ubuntu-12-0-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda
```

### reference
https://www.bilibili.com/read/cv14608547
